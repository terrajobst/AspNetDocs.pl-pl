---
uid: visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
title: 'Wskazówki dotyczące laboratorium: jeden ASP.NET: Integrowanie formularzy sieci Web ASP.NET, MVC i Web API | Microsoft Docs'
author: rick-anderson
description: ASP.NET to struktura służąca do kompilowania witryn sieci Web, aplikacji i usług przy użyciu specjalistycznych technologii, takich jak MVC, Web API i inne. Z rozszerzeniem ASP.NET h...
ms.author: riande
ms.date: 07/16/2014
ms.assetid: 4fe2558d-67cc-4d12-a5c1-6fb9f6f16137
msc.legacyurl: /visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
msc.type: authoredcontent
ms.openlocfilehash: 165d104b5d3ef3281af449cc8673ad96f531d628
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78623201"
---
# <a name="hands-on-lab-one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api"></a><span data-ttu-id="e6d28-104">Ćwiczenia praktyczne: One ASP.NET: integrowanie wzorców ASP.NET Web Forms, MVC i Web API</span><span class="sxs-lookup"><span data-stu-id="e6d28-104">Hands On Lab: One ASP.NET: Integrating ASP.NET Web Forms, MVC and Web API</span></span>

<span data-ttu-id="e6d28-105">przez [zespół Camp sieci Web](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="e6d28-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="e6d28-106">Pobierz zestaw szkoleniowy dla sieci Web Camp</span><span class="sxs-lookup"><span data-stu-id="e6d28-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

> <span data-ttu-id="e6d28-107">ASP.NET to struktura służąca do kompilowania witryn sieci Web, aplikacji i usług przy użyciu specjalistycznych technologii, takich jak MVC, Web API i inne.</span><span class="sxs-lookup"><span data-stu-id="e6d28-107">ASP.NET is a framework for building Web sites, apps and services using specialized technologies such as MVC, Web API and others.</span></span> <span data-ttu-id="e6d28-108">Wraz z ASP.NET ekspansji pojawiły się od momentu jego utworzenia, a wyrażone w tym przypadku muszą mieć zintegrowane technologie, podczas pracy w kierunku **jednego ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="e6d28-108">With the expansion ASP.NET has seen since its creation and the expressed need to have these technologies integrated, there have been recent efforts in working towards **One ASP.NET**.</span></span>
> 
> <span data-ttu-id="e6d28-109">W Visual Studio 2013 wprowadzono nowy ujednolicony system projektu, który umożliwia tworzenie aplikacji i używanie wszystkich technologii ASP.NET w jednym projekcie.</span><span class="sxs-lookup"><span data-stu-id="e6d28-109">Visual Studio 2013 introduces a new unified project system which lets you build an application and use all the ASP.NET technologies in one project.</span></span> <span data-ttu-id="e6d28-110">Ta funkcja eliminuje konieczność wyboru jednej technologii na początku projektu i naklejenia z nią, a zamiast tego zaleca użycie wielu platform ASP.NET w ramach jednego projektu.</span><span class="sxs-lookup"><span data-stu-id="e6d28-110">This feature eliminates the need to pick one technology at the start of a project and stick with it, and instead encourages the use of multiple ASP.NET frameworks within one project.</span></span>
> 
> <span data-ttu-id="e6d28-111">Wszystkie przykładowe kod i fragmenty kodu są zawarte w zestawie szkoleń w sieci Web Camp, które są dostępne w [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="e6d28-111">All sample code and snippets are included in the Web Camps Training Kit, available at [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit).</span></span>

<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="e6d28-112">Omówienie</span><span class="sxs-lookup"><span data-stu-id="e6d28-112">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="e6d28-113">Cele</span><span class="sxs-lookup"><span data-stu-id="e6d28-113">Objectives</span></span>

<span data-ttu-id="e6d28-114">W tym ćwiczeniu dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="e6d28-114">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="e6d28-115">Tworzenie witryny sieci Web opartej na **jednym** typie projektu ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e6d28-115">Create a Web site based on the **One ASP.NET** project type</span></span>
- <span data-ttu-id="e6d28-116">Korzystanie z różnych platform **ASP.NET** , takich jak **MVC** i **Web API** w tym samym projekcie</span><span class="sxs-lookup"><span data-stu-id="e6d28-116">Use different **ASP.NET** frameworks like **MVC** and **Web API** in the same project</span></span>
- <span data-ttu-id="e6d28-117">Zidentyfikuj główne składniki aplikacji **ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="e6d28-117">Identify the main components of an **ASP.NET** application</span></span>
- <span data-ttu-id="e6d28-118">Korzystanie z platformy tworzenia **szkieletów ASP.NET** do automatycznego tworzenia kontrolerów i widoków w celu wykonywania operacji CRUD na podstawie klas modelu</span><span class="sxs-lookup"><span data-stu-id="e6d28-118">Take advantage of the **ASP.NET Scaffolding** framework to automatically create Controllers and Views to perform CRUD operations based on your model classes</span></span>
- <span data-ttu-id="e6d28-119">Udostępnianie tego samego zestawu informacji w formatach maszynowych i czytelnych przez człowieka przy użyciu odpowiedniego narzędzia dla każdego zadania</span><span class="sxs-lookup"><span data-stu-id="e6d28-119">Expose the same set of information in machine- and human-readable formats using the right tool for each job</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="e6d28-120">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="e6d28-120">Prerequisites</span></span>

<span data-ttu-id="e6d28-121">Następujące czynności są wymagane do wykonania tego laboratorium praktycznego:</span><span class="sxs-lookup"><span data-stu-id="e6d28-121">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="e6d28-122">[Visual Studio Express 2013 dla sieci Web](https://www.microsoft.com/visualstudio/) lub nowszego</span><span class="sxs-lookup"><span data-stu-id="e6d28-122">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="e6d28-123">Visual Studio 2013 Update 1</span><span class="sxs-lookup"><span data-stu-id="e6d28-123">Visual Studio 2013 Update 1</span></span>](https://go.microsoft.com/fwlink/?LinkId=301714)

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="e6d28-124">Konfigurowanie</span><span class="sxs-lookup"><span data-stu-id="e6d28-124">Setup</span></span>

<span data-ttu-id="e6d28-125">Aby można było uruchomić ćwiczenia w tym ćwiczeniu, należy najpierw skonfigurować środowisko.</span><span class="sxs-lookup"><span data-stu-id="e6d28-125">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="e6d28-126">Otwórz Eksploratora Windows i przejdź do folderu **źródłowego** laboratorium.</span><span class="sxs-lookup"><span data-stu-id="e6d28-126">Open Windows Explorer and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="e6d28-127">Kliknij prawym przyciskiem myszy pozycję **Setup. cmd** i wybierz polecenie **Uruchom jako administrator** , aby uruchomić proces instalacji, który skonfiguruje środowisko i zainstaluje fragmenty kodu programu Visual Studio dla tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="e6d28-127">Right-click on **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="e6d28-128">Jeśli zostanie wyświetlone okno dialogowe Kontrola konta użytkownika, potwierdź akcję, aby wykonać operację.</span><span class="sxs-lookup"><span data-stu-id="e6d28-128">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="e6d28-129">Upewnij się, że wszystkie zależności dla tego laboratorium zostały sprawdzone przed uruchomieniem Instalatora.</span><span class="sxs-lookup"><span data-stu-id="e6d28-129">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>

<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="e6d28-130">Używanie fragmentów kodu</span><span class="sxs-lookup"><span data-stu-id="e6d28-130">Using the Code Snippets</span></span>

<span data-ttu-id="e6d28-131">W całym dokumencie laboratoryjnym pojawi się monit o wstawienie bloków kodu.</span><span class="sxs-lookup"><span data-stu-id="e6d28-131">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="e6d28-132">Dla wygody większość tego kodu jest udostępniana jako fragmenty Visual Studio Code, do których można uzyskać dostęp z poziomu Visual Studio 2013, aby uniknąć konieczności ręcznego dodawania go.</span><span class="sxs-lookup"><span data-stu-id="e6d28-132">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="e6d28-133">Każdemu z nich towarzyszy rozpoczęcie rozwiązanie znajdujące się w folderze **BEGIN** w ćwiczeniu, który umożliwia wykonywanie poszczególnych czynności niezależnie od innych.</span><span class="sxs-lookup"><span data-stu-id="e6d28-133">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="e6d28-134">Należy pamiętać, że fragmenty kodu dodawane podczas wykonywania nie są dostępne w tych rozwiązaniach i mogą nie działać do czasu ukończenia ćwiczenia.</span><span class="sxs-lookup"><span data-stu-id="e6d28-134">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="e6d28-135">Wewnątrz kodu źródłowego dla ćwiczenia znajdziesz również folder **końcowy** zawierający rozwiązanie programu Visual Studio z kodem, który wynika z wykonania czynności w odpowiednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="e6d28-135">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="e6d28-136">Te rozwiązania można wykorzystać jako wskazówkę, jeśli potrzebujesz dodatkowej pomocy podczas pracy w ramach tego praktycznego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="e6d28-136">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>

---

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="e6d28-137">Symulacyjn</span><span class="sxs-lookup"><span data-stu-id="e6d28-137">Exercises</span></span>

<span data-ttu-id="e6d28-138">To laboratorium praktyczne obejmuje następujące ćwiczenia:</span><span class="sxs-lookup"><span data-stu-id="e6d28-138">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="e6d28-139">Tworzenie nowego projektu formularzy sieci Web</span><span class="sxs-lookup"><span data-stu-id="e6d28-139">Creating a New Web Forms Project</span></span>](#Exercise1)
2. [<span data-ttu-id="e6d28-140">Tworzenie kontrolera MVC przy użyciu szkieletu</span><span class="sxs-lookup"><span data-stu-id="e6d28-140">Creating an MVC Controller Using Scaffolding</span></span>](#Exercise2)
3. [<span data-ttu-id="e6d28-141">Tworzenie kontrolera interfejsu API sieci Web przy użyciu szkieletu</span><span class="sxs-lookup"><span data-stu-id="e6d28-141">Creating a Web API Controller Using Scaffolding</span></span>](#Exercise3)

<span data-ttu-id="e6d28-142">Szacowany czas wykonywania tego laboratorium: **60 minut**</span><span class="sxs-lookup"><span data-stu-id="e6d28-142">Estimated time to complete this lab: **60 minutes**</span></span>

> [!NOTE]
> <span data-ttu-id="e6d28-143">Po pierwszym uruchomieniu programu Visual Studio należy wybrać jedną z wstępnie zdefiniowanych kolekcji ustawień.</span><span class="sxs-lookup"><span data-stu-id="e6d28-143">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="e6d28-144">Każda wstępnie zdefiniowana kolekcja jest zaprojektowana tak, aby była zgodna z konkretnym stylem deweloperskim i określa układy okien, zachowanie edytora, fragmenty kodu IntelliSense i opcje okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="e6d28-144">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="e6d28-145">Procedury przedstawione w tym laboratorium opisują akcje niezbędne do wykonania danego zadania w programie Visual Studio, gdy jest używana **Ogólna kolekcja ustawień deweloperskich** .</span><span class="sxs-lookup"><span data-stu-id="e6d28-145">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="e6d28-146">W przypadku wybrania innej kolekcji ustawień dla środowiska programistycznego mogą wystąpić różnice w czynnościach, które należy wziąć pod uwagę.</span><span class="sxs-lookup"><span data-stu-id="e6d28-146">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>

<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-new-web-forms-project"></a><span data-ttu-id="e6d28-147">Ćwiczenie 1: Tworzenie nowego projektu formularzy sieci Web</span><span class="sxs-lookup"><span data-stu-id="e6d28-147">Exercise 1: Creating a New Web Forms Project</span></span>

<span data-ttu-id="e6d28-148">W tym ćwiczeniu zostanie utworzona nowa witryna formularzy sieci Web w Visual Studio 2013 przy użyciu **jednego ASP.NET** ujednoliconego środowiska projektowego, dzięki któremu można łatwo zintegrować składniki Web Forms, MVC i Web API w tej samej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e6d28-148">In this exercise you will create a new Web Forms site in Visual Studio 2013 using the **One ASP.NET** unified project experience, which will allow you to easily integrate Web Forms, MVC and Web API components in the same application.</span></span> <span data-ttu-id="e6d28-149">Następnie zostanie zbadane wygenerowane rozwiązanie i zidentyfikowanie jego części, a wreszcie zostanie wyświetlona witryna sieci Web w działaniu.</span><span class="sxs-lookup"><span data-stu-id="e6d28-149">You will then explore the generated solution and identify its parts, and finally you will see the Web site in action.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-a-new-site-using-the-one-aspnet-experience"></a><span data-ttu-id="e6d28-150">Zadanie 1 — Tworzenie nowej witryny przy użyciu jednego środowiska ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e6d28-150">Task 1 – Creating a New Site Using the One ASP.NET Experience</span></span>

<span data-ttu-id="e6d28-151">W tym zadaniu rozpocznie się tworzenie nowej witryny sieci Web w programie Visual Studio w oparciu o typ projektu **ASP.NET** .</span><span class="sxs-lookup"><span data-stu-id="e6d28-151">In this task you will start creating a new Web site in Visual Studio based on the **One ASP.NET** project type.</span></span> <span data-ttu-id="e6d28-152">**Jedna ASP.NET** łączy wszystkie technologie ASP.NET i pozwala na ich mieszanie i dopasowanie odpowiednio do potrzeb.</span><span class="sxs-lookup"><span data-stu-id="e6d28-152">**One ASP.NET** unifies all ASP.NET technologies and gives you the option to mix and match them as desired.</span></span> <span data-ttu-id="e6d28-153">Następnie zostaną rozpoznane różne składniki formularzy Web Forms, MVC i Web API, które znajdują się obok siebie w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e6d28-153">You will then recognize the different components of Web Forms, MVC and Web API that live side by side within your application.</span></span>

1. <span data-ttu-id="e6d28-154">Otwórz **Visual Studio Express 2013 dla sieci Web** i wybierz pozycję **plik | Nowy projekt...** , aby rozpocząć nowe rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="e6d28-154">Open **Visual Studio Express 2013 for Web** and select **File | New Project...** to start a new solution.</span></span>

    ![Tworzenie nowego projektu](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image1.png)

    <span data-ttu-id="e6d28-156">*Tworzenie nowego projektu*</span><span class="sxs-lookup"><span data-stu-id="e6d28-156">*Creating a New Project*</span></span>
2. <span data-ttu-id="e6d28-157">W oknie dialogowym **Nowy projekt** wybierz pozycję **aplikacja sieci Web ASP.NET** w obszarze **Wizualizacja C# |** Upewnij się, że jest zaznaczona **.NET Framework 4,5** .</span><span class="sxs-lookup"><span data-stu-id="e6d28-157">In the **New Project** dialog box, select **ASP.NET Web Application** under the **Visual C# | Web** tab, and make sure **.NET Framework 4.5** is selected.</span></span> <span data-ttu-id="e6d28-158">Nazwij projekt *MyHybridSite*, wybierz **lokalizację** i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="e6d28-158">Name the project *MyHybridSite*, choose a **Location** and click **OK**.</span></span>

    ![Nowy projekt aplikacji sieci Web ASP.NET](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image2.png)

    <span data-ttu-id="e6d28-160">*Tworzenie nowego projektu aplikacji sieci Web ASP.NET*</span><span class="sxs-lookup"><span data-stu-id="e6d28-160">*Creating a new ASP.NET Web Application project*</span></span>
3. <span data-ttu-id="e6d28-161">W oknie dialogowym **Nowy projekt ASP.NET** wybierz szablon **formularze sieci Web** i wybierz opcje **MVC** i **Web API** .</span><span class="sxs-lookup"><span data-stu-id="e6d28-161">In the **New ASP.NET Project** dialog box, select the **Web Forms** template and select the **MVC** and **Web API** options.</span></span> <span data-ttu-id="e6d28-162">Upewnij się również, że opcja **uwierzytelniania** jest ustawiona na **konta poszczególnych użytkowników**.</span><span class="sxs-lookup"><span data-stu-id="e6d28-162">Also, make sure that the **Authentication** option is set to **Individual User Accounts**.</span></span> <span data-ttu-id="e6d28-163">Kliknij przycisk **OK** , aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="e6d28-163">Click **OK** to continue.</span></span>

    ![Tworzenie nowego projektu za pomocą szablonu formularzy sieci Web, w tym składnika Web API i składników MVC](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image3.png)

    <span data-ttu-id="e6d28-165">*Tworzenie nowego projektu za pomocą szablonu formularzy sieci Web, w tym składnika Web API i składników MVC*</span><span class="sxs-lookup"><span data-stu-id="e6d28-165">*Creating a new project with the Web Forms template, including Web API and MVC components*</span></span>
4. <span data-ttu-id="e6d28-166">Teraz możesz zapoznać się ze strukturą wygenerowanego rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="e6d28-166">You can now explore the structure of the generated solution.</span></span>

    ![Eksplorowanie wygenerowanego rozwiązania](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image4.png)

    <span data-ttu-id="e6d28-168">*Eksplorowanie wygenerowanego rozwiązania*</span><span class="sxs-lookup"><span data-stu-id="e6d28-168">*Exploring the generated solution*</span></span>

    1. <span data-ttu-id="e6d28-169">**Konto:** Ten folder zawiera strony formularza sieci Web, które umożliwiają rejestrację i logowanie do kont użytkowników aplikacji oraz zarządzanie nimi.</span><span class="sxs-lookup"><span data-stu-id="e6d28-169">**Account:** This folder contains the Web Form pages to register, log in to and manage the application's user accounts.</span></span> <span data-ttu-id="e6d28-170">Ten folder jest dodawany w przypadku wybrania opcji uwierzytelnianie **poszczególnych kont użytkowników** podczas konfiguracji szablonu projektu formularzy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="e6d28-170">This folder is added when the **Individual User Accounts** authentication option is selected during the configuration of the Web Forms project template.</span></span>
    2. <span data-ttu-id="e6d28-171">**Modele:** Ten folder będzie zawierać klasy, które reprezentują dane aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e6d28-171">**Models:** This folder will contain the classes that represent your application data.</span></span>
    3. <span data-ttu-id="e6d28-172">**Kontrolery** i **widoki**: te foldery są wymagane dla składników **interfejsu API sieci Web** **ASP.NET MVC** i ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e6d28-172">**Controllers** and **Views**: These folders are required for the **ASP.NET MVC** and **ASP.NET Web API** components.</span></span> <span data-ttu-id="e6d28-173">W następnych ćwiczeniach znajdziesz technologie MVC i interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="e6d28-173">You will explore the MVC and Web API technologies in the next exercises.</span></span>
    4. <span data-ttu-id="e6d28-174">**Default. aspx**, **Contact. aspx** i **Informacje o plikach. aspx** są wstępnie zdefiniowanymi stronami formularzy sieci Web, których można użyć jako punktów początkowych w celu utworzenia stron specyficznych dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e6d28-174">The **Default.aspx**, **Contact.aspx** and **About.aspx** files are pre-defined Web Form pages that you can use as starting points to build the pages specific to your application.</span></span> <span data-ttu-id="e6d28-175">Logika programowania tych plików znajduje się w osobnym pliku, zwanym &quot;&quot; pliku, który ma &quot;. aspx. vb&quot; lub &quot;. aspx.cs&quot; rozszerzenia (w zależności od używanego języka).</span><span class="sxs-lookup"><span data-stu-id="e6d28-175">The programming logic of those files resides in a separate file referred to as the &quot;code-behind&quot; file, which has an &quot;.aspx.vb&quot; or &quot;.aspx.cs&quot; extension (depending on the language used).</span></span> <span data-ttu-id="e6d28-176">Logika związany z kodem jest uruchamiana na serwerze i dynamicznie tworzy dane wyjściowe HTML dla strony.</span><span class="sxs-lookup"><span data-stu-id="e6d28-176">The code-behind logic runs on the server and dynamically produces the HTML output for your page.</span></span>
    5. <span data-ttu-id="e6d28-177">Strony **site. Master** i **site. Mobile. Master** definiują wygląd i działanie oraz standardowe zachowanie wszystkich stron w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e6d28-177">The **Site.Master** and **Site.Mobile.Master** pages define the look and feel and the standard behavior of all the pages in the application.</span></span>
5. <span data-ttu-id="e6d28-178">Kliknij dwukrotnie plik **default. aspx** , aby poznać zawartość strony.</span><span class="sxs-lookup"><span data-stu-id="e6d28-178">Double-click the **Default.aspx** file to explore the content of the page.</span></span>

    ![Eksplorowanie domyślnej strony. aspx](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image5.png)

    <span data-ttu-id="e6d28-180">*Eksplorowanie domyślnej strony. aspx*</span><span class="sxs-lookup"><span data-stu-id="e6d28-180">*Exploring the Default.aspx page*</span></span>

    > [!NOTE]
    > <span data-ttu-id="e6d28-181">Dyrektywa **Page** w górnej części pliku definiuje atrybuty strony formularzy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="e6d28-181">The **Page** directive at the top of the file defines the attributes of the Web Forms page.</span></span> <span data-ttu-id="e6d28-182">Na przykład atrybut **MasterPageFile** określa ścieżkę do strony głównej — w tym przypadku strona *site. Master* i atrybut **Inherits** definiuje klasę powiązany kod dla strony do dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="e6d28-182">For example, the **MasterPageFile** attribute specifies the path to the master page -in this case, the *Site.Master* page- and the **Inherits** attribute defines the code-behind class for the page to inherit.</span></span> <span data-ttu-id="e6d28-183">Ta klasa znajduje się w pliku określonym przez atrybut **CodeBehind** .</span><span class="sxs-lookup"><span data-stu-id="e6d28-183">This class is located in the file determined by the **CodeBehind** attribute.</span></span>
    > 
    > <span data-ttu-id="e6d28-184">Formant **ASP: zawartość** przechowuje rzeczywistą zawartość strony (tekst, znacznik i kontrolki) i jest zamapowana na kontrolkę **ASP: ContentPlaceHolder** na stronie wzorcowej.</span><span class="sxs-lookup"><span data-stu-id="e6d28-184">The **asp:Content** control holds the actual content of the page (text, markup and controls) and is mapped to a **asp:ContentPlaceHolder** control on the master page.</span></span> <span data-ttu-id="e6d28-185">W takim przypadku zawartość strony będzie renderowana wewnątrz kontrolki *kontrolka mainContent* zdefiniowanej na stronie *site. Master* .</span><span class="sxs-lookup"><span data-stu-id="e6d28-185">In this case, the page content will be rendered inside the *MainContent* control defined in the *Site.Master* page.</span></span>
6. <span data-ttu-id="e6d28-186">Rozwiń węzeł **aplikacji\_Start** i zwróć uwagę na plik **WebApiConfig.cs** .</span><span class="sxs-lookup"><span data-stu-id="e6d28-186">Expand the **App\_Start** folder and notice the **WebApiConfig.cs** file.</span></span> <span data-ttu-id="e6d28-187">Program Visual Studio uwzględniał ten plik w wygenerowanym rozwiązaniu, ponieważ dołączono internetowy interfejs API podczas konfigurowania projektu przy użyciu jednego szablonu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e6d28-187">Visual Studio included that file in the generated solution because you included Web API when configuring your project with the One ASP.NET template.</span></span>
7. <span data-ttu-id="e6d28-188">Otwórz plik **WebApiConfig.cs** .</span><span class="sxs-lookup"><span data-stu-id="e6d28-188">Open the **WebApiConfig.cs** file.</span></span> <span data-ttu-id="e6d28-189">W klasie *WebApiConfig* znajduje się konfiguracja skojarzona z interfejsem API sieci Web, która mapuje trasy protokołu HTTP na **Kontrolery interfejsu API sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="e6d28-189">In the *WebApiConfig* class you will find the configuration associated with Web API, which maps HTTP routes to **Web API controllers**.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample1.cs)]
8. <span data-ttu-id="e6d28-190">Otwórz plik **RouteConfig.cs** .</span><span class="sxs-lookup"><span data-stu-id="e6d28-190">Open the **RouteConfig.cs** file.</span></span> <span data-ttu-id="e6d28-191">Wewnątrz metody *RegisterRoutes* znajduje się konfiguracja skojarzona z MVC, która mapuje trasy protokołu HTTP na **Kontrolery MVC**.</span><span class="sxs-lookup"><span data-stu-id="e6d28-191">Inside the *RegisterRoutes* method you will find the configuration associated with MVC, which maps HTTP routes to **MVC controllers**.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample2.cs)]

<a id="Ex1Task2"></a>
#### <a name="task-2--running-the-solution"></a><span data-ttu-id="e6d28-192">Zadanie 2 — Uruchamianie rozwiązania</span><span class="sxs-lookup"><span data-stu-id="e6d28-192">Task 2 – Running the Solution</span></span>

<span data-ttu-id="e6d28-193">W tym zadaniu zostanie uruchomione wygenerowane rozwiązanie, zbadasz aplikację i niektóre jej funkcje, takie jak ponowne zapisywanie adresów URL i uwierzytelnianie wbudowane.</span><span class="sxs-lookup"><span data-stu-id="e6d28-193">In this task you will run the generated solution, explore the app and some of its features, like URL rewriting and built-in authentication.</span></span>

1. <span data-ttu-id="e6d28-194">Aby uruchomić rozwiązanie, naciśnij klawisz **F5** lub kliknij przycisk **Start** znajdujący się na pasku narzędzi.</span><span class="sxs-lookup"><span data-stu-id="e6d28-194">To run the solution, press **F5** or click the **Start** button located on the toolbar.</span></span> <span data-ttu-id="e6d28-195">Strona główna aplikacji powinna być otwarta w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="e6d28-195">The application home page should open in the browser.</span></span>

    ![Uruchamianie rozwiązania](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image6.png)
2. <span data-ttu-id="e6d28-197">Sprawdź, czy strony formularzy sieci Web są wywoływane.</span><span class="sxs-lookup"><span data-stu-id="e6d28-197">Verify that the Web Forms pages are being invoked.</span></span> <span data-ttu-id="e6d28-198">W tym celu Dołącz **/Contact.aspx** do adresu URL na pasku adresu i naciśnij klawisz **Enter**.</span><span class="sxs-lookup"><span data-stu-id="e6d28-198">To do this, append **/contact.aspx** to the URL in the address bar and press **Enter**.</span></span>

    ![Przyjazne adresy URL](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image7.png)

    <span data-ttu-id="e6d28-200">*Przyjazne adresy URL*</span><span class="sxs-lookup"><span data-stu-id="e6d28-200">*Friendly URLs*</span></span>

    > [!NOTE]
    > <span data-ttu-id="e6d28-201">Jak widać, adres URL zmieni się na **/Contact**.</span><span class="sxs-lookup"><span data-stu-id="e6d28-201">As you can see, the URL changes to **/contact**.</span></span> <span data-ttu-id="e6d28-202">Począwszy od **ASP.NET 4**, możliwości routingu adresów URL zostały dodane do formularzy sieci Web, dzięki czemu można pisać adresy URL, takie jak *[http://www.mysite.com/products/software](http://www.mysite.com/products/software)* , a nie *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)* .</span><span class="sxs-lookup"><span data-stu-id="e6d28-202">Starting from **ASP.NET 4**, URL routing capabilities were added to Web Forms, so you can write URLs like *[http://www.mysite.com/products/software](http://www.mysite.com/products/software)* instead of *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*.</span></span> <span data-ttu-id="e6d28-203">Aby uzyskać więcej informacji, zobacz [Routing adresów URL](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).</span><span class="sxs-lookup"><span data-stu-id="e6d28-203">For more information refer to [URL Routing](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).</span></span>
3. <span data-ttu-id="e6d28-204">Teraz zostanie zbadany przepływ uwierzytelniania zintegrowany z aplikacją.</span><span class="sxs-lookup"><span data-stu-id="e6d28-204">You will now explore the authentication flow integrated into the application.</span></span> <span data-ttu-id="e6d28-205">W tym celu kliknij pozycję **zarejestruj** w prawym górnym rogu strony.</span><span class="sxs-lookup"><span data-stu-id="e6d28-205">To do this, click **Register** in the upper-right corner of the page.</span></span>

    ![Rejestrowanie nowego użytkownika](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image8.png)

    <span data-ttu-id="e6d28-207">*Rejestrowanie nowego użytkownika*</span><span class="sxs-lookup"><span data-stu-id="e6d28-207">*Registering a new user*</span></span>
4. <span data-ttu-id="e6d28-208">Na stronie **Rejestr** wprowadź **nazwę użytkownika** i **hasło**, a następnie kliknij pozycję **zarejestruj**.</span><span class="sxs-lookup"><span data-stu-id="e6d28-208">In the **Register** page, enter a **User name** and **Password**, and then click **Register**.</span></span>

    ![Strona rejestracji](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image9.png)

    <span data-ttu-id="e6d28-210">*Strona rejestracji*</span><span class="sxs-lookup"><span data-stu-id="e6d28-210">*Register page*</span></span>
5. <span data-ttu-id="e6d28-211">Aplikacja rejestruje nowe konto, a użytkownik jest uwierzytelniany.</span><span class="sxs-lookup"><span data-stu-id="e6d28-211">The application registers the new account, and the user is authenticated.</span></span>

    ![Użytkownik uwierzytelniony](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image10.png)

    <span data-ttu-id="e6d28-213">*Użytkownik uwierzytelniony*</span><span class="sxs-lookup"><span data-stu-id="e6d28-213">*User authenticated*</span></span>
6. <span data-ttu-id="e6d28-214">Wróć do programu Visual Studio i naciśnij klawisze **Shift + F5** , aby zatrzymać debugowanie.</span><span class="sxs-lookup"><span data-stu-id="e6d28-214">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-creating-an-mvc-controller-using-scaffolding"></a><span data-ttu-id="e6d28-215">Ćwiczenie 2: Tworzenie kontrolera MVC przy użyciu szkieletu</span><span class="sxs-lookup"><span data-stu-id="e6d28-215">Exercise 2: Creating an MVC Controller Using Scaffolding</span></span>

<span data-ttu-id="e6d28-216">W tym ćwiczeniu wykorzystasz platformę tworzenia szkieletów ASP.NET zapewnianą przez program Visual Studio, aby utworzyć kontroler ASP.NET MVC 5 z akcjami i widokami Razor do wykonywania operacji CRUD, bez konieczności pisania pojedynczego wiersza kodu.</span><span class="sxs-lookup"><span data-stu-id="e6d28-216">In this exercise you will take advantage of the ASP.NET Scaffolding framework provided by Visual Studio to create an ASP.NET MVC 5 controller with actions and Razor views to perform CRUD operations, without writing a single line of code.</span></span> <span data-ttu-id="e6d28-217">Proces tworzenia szkieletu będzie używać Entity Framework Code First do wygenerowania kontekstu danych i schematu bazy danych w bazie danych SQL.</span><span class="sxs-lookup"><span data-stu-id="e6d28-217">The scaffolding process will use Entity Framework Code First to generate the data context and the database schema in the SQL database.</span></span>

<span data-ttu-id="e6d28-218">**Informacje o Entity Framework Code First**</span><span class="sxs-lookup"><span data-stu-id="e6d28-218">**About Entity Framework Code First**</span></span>

<span data-ttu-id="e6d28-219">Entity Framework (EF) to Mapowanie obiektowo-relacyjne (ORM), które umożliwia tworzenie aplikacji do uzyskiwania dostępu do danych przez Programowanie przy użyciu modelu aplikacji koncepcyjnej zamiast programowania bezpośrednio przy użyciu relacyjnego schematu magazynu.</span><span class="sxs-lookup"><span data-stu-id="e6d28-219">Entity Framework (EF) is an object-relational mapper (ORM) that enables you to create data access applications by programming with a conceptual application model instead of programming directly using a relational storage schema.</span></span>

<span data-ttu-id="e6d28-220">Przepływ pracy modelowania Entity Framework Code First umożliwia korzystanie z własnych klas domeny do reprezentowania modelu, na którym opiera się program Dr podczas wykonywania zapytań, śledzenia zmian i aktualizowania funkcji.</span><span class="sxs-lookup"><span data-stu-id="e6d28-220">The Entity Framework Code First modeling workflow allows you to use your own domain classes to represent the model that EF relies on when performing querying, change-tracking and updating functions.</span></span> <span data-ttu-id="e6d28-221">Korzystając z przepływu pracy tworzenia Code First, nie musisz rozpoczynać swojej aplikacji, tworząc bazę danych lub określając schemat.</span><span class="sxs-lookup"><span data-stu-id="e6d28-221">Using the Code First development workflow, you do not need to begin your application by creating a database or specifying a schema.</span></span> <span data-ttu-id="e6d28-222">Zamiast tego można napisać standardowe klasy .NET, które definiują najbardziej odpowiednie obiekty modelu domeny dla aplikacji, a Entity Framework utworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="e6d28-222">Instead, you can write standard .NET classes that define the most appropriate domain model objects for your application, and Entity Framework will create the database for you.</span></span>

> [!NOTE]
> <span data-ttu-id="e6d28-223">Więcej informacji na temat Entity Framework można znaleźć [tutaj](../../../entity-framework.md).</span><span class="sxs-lookup"><span data-stu-id="e6d28-223">You can learn more about Entity Framework [here](../../../entity-framework.md).</span></span>

<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-new-model"></a><span data-ttu-id="e6d28-224">Zadanie 1 — Tworzenie nowego modelu</span><span class="sxs-lookup"><span data-stu-id="e6d28-224">Task 1 – Creating a New Model</span></span>

<span data-ttu-id="e6d28-225">Teraz zdefiniujemy klasy **Person** , która będzie modelem używanym przez proces tworzenia szkieletu, aby utworzyć kontroler MVC i widoki.</span><span class="sxs-lookup"><span data-stu-id="e6d28-225">You will now define a **Person** class, which will be the model used by the scaffolding process to create the MVC controller and the views.</span></span> <span data-ttu-id="e6d28-226">Zacznij od utworzenia klasy modelu **osoby** i CRUD operacje w kontrolerze zostaną automatycznie utworzone przy użyciu funkcji tworzenia szkieletów.</span><span class="sxs-lookup"><span data-stu-id="e6d28-226">You will start by creating a **Person** model class, and the CRUD operations in the controller will be automatically created using scaffolding features.</span></span>

1. <span data-ttu-id="e6d28-227">Otwórz **Visual Studio Express 2013 dla sieci Web** i rozwiązania **MyHybridSite. sln** znajdującego się w folderze **Source/Ex2-MvcScaffolding/BEGIN** .</span><span class="sxs-lookup"><span data-stu-id="e6d28-227">Open **Visual Studio Express 2013 for Web** and the **MyHybridSite.sln** solution located in the **Source/Ex2-MvcScaffolding/Begin** folder.</span></span> <span data-ttu-id="e6d28-228">Alternatywnie możesz kontynuować z rozwiązaniem uzyskanym w poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="e6d28-228">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>
2. <span data-ttu-id="e6d28-229">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder **modele** projektu **MyHybridSite** i wybierz polecenie **Dodaj | Klasa...** .</span><span class="sxs-lookup"><span data-stu-id="e6d28-229">In **Solution Explorer**, right-click the **Models** folder of the **MyHybridSite** project and select **Add | Class...**.</span></span>

    ![Dodawanie klasy modelu osoby](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image11.png)

    <span data-ttu-id="e6d28-231">*Dodawanie klasy modelu osoby*</span><span class="sxs-lookup"><span data-stu-id="e6d28-231">*Adding the Person model class*</span></span>
3. <span data-ttu-id="e6d28-232">W oknie dialogowym **Dodaj nowy element** nazwij plik *Person.cs* i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="e6d28-232">In the **Add New Item** dialog box, name the file *Person.cs* and click **Add**.</span></span>

    ![Tworzenie klasy modelu osoby](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image12.png)

    <span data-ttu-id="e6d28-234">*Tworzenie klasy modelu osoby*</span><span class="sxs-lookup"><span data-stu-id="e6d28-234">*Creating the Person model class*</span></span>
4. <span data-ttu-id="e6d28-235">Zastąp zawartość pliku **Person.cs** następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="e6d28-235">Replace the content of the **Person.cs** file with the following code.</span></span> <span data-ttu-id="e6d28-236">Naciśnij **kombinację klawiszy Ctrl + S** , aby zapisać zmiany.</span><span class="sxs-lookup"><span data-stu-id="e6d28-236">Press **CTRL + S** to save the changes.</span></span>

    <span data-ttu-id="e6d28-237">(Fragment kodu- *BringingTogetherOneAspNet-Ex2-PersonClass*)</span><span class="sxs-lookup"><span data-stu-id="e6d28-237">(Code Snippet - *BringingTogetherOneAspNet - Ex2 - PersonClass*)</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample3.cs)]
5. <span data-ttu-id="e6d28-238">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt **MyHybridSite** i wybierz polecenie **Kompiluj**lub naciśnij **klawisze CTRL + SHIFT + B** , aby skompilować projekt.</span><span class="sxs-lookup"><span data-stu-id="e6d28-238">In **Solution Explorer**, right-click the **MyHybridSite** project and select **Build**, or press **CTRL + SHIFT + B** to build the project.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-an-mvc-controller"></a><span data-ttu-id="e6d28-239">Zadanie 2 — Tworzenie kontrolera MVC</span><span class="sxs-lookup"><span data-stu-id="e6d28-239">Task 2 – Creating an MVC Controller</span></span>

<span data-ttu-id="e6d28-240">Teraz, gdy jest tworzony model **osoby** , użyjesz szkieletu ASP.NET MVC z Entity Framework, aby utworzyć akcje i widoki kontrolera CRUD dla **osoby**.</span><span class="sxs-lookup"><span data-stu-id="e6d28-240">Now that the **Person** model is created, you will use ASP.NET MVC scaffolding with Entity Framework to create the CRUD controller actions and views for **Person**.</span></span>

1. <span data-ttu-id="e6d28-241">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder **controllers (kontrolery** ) projektu **MyHybridSite** i wybierz polecenie **Dodaj | Nowy element szkieletowy..** .</span><span class="sxs-lookup"><span data-stu-id="e6d28-241">In **Solution Explorer**, right-click the **Controllers** folder of the **MyHybridSite** project and select **Add | New Scaffolded Item...**.</span></span>

    ![Tworzenie nowego kontrolera szkieletowego](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image13.png)

    <span data-ttu-id="e6d28-243">*Tworzenie nowego kontrolera szkieletowego*</span><span class="sxs-lookup"><span data-stu-id="e6d28-243">*Creating a new Scaffolded Controller*</span></span>
2. <span data-ttu-id="e6d28-244">W oknie dialogowym **Dodawanie szkieletu** wybierz **kontroler MVC 5 z widokami, używając Entity Framework** a następnie kliknij przycisk **Dodaj.**</span><span class="sxs-lookup"><span data-stu-id="e6d28-244">In the **Add Scaffold** dialog box, select **MVC 5 Controller with views, using Entity Framework** and then click **Add.**</span></span>

    ![Wybieranie kontrolera MVC 5 z widokami i Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image14.png)

    <span data-ttu-id="e6d28-246">*Wybieranie kontrolera MVC 5 z widokami i Entity Framework*</span><span class="sxs-lookup"><span data-stu-id="e6d28-246">*Selecting MVC 5 Controller with views and Entity Framework*</span></span>
3. <span data-ttu-id="e6d28-247">Ustaw *MvcPersonController* jako **nazwę kontrolera**, wybierz opcję **Użyj akcji kontrolerów asynchronicznych** i wybierz **osobę (MyHybridSite. models)** jako **klasę modelu**.</span><span class="sxs-lookup"><span data-stu-id="e6d28-247">Set *MvcPersonController* as the **Controller name**, select the **Use async controller actions** option and select **Person (MyHybridSite.Models)** as the **Model class**.</span></span>

    ![Dodawanie kontrolera MVC z szkieletem](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image15.png)

    <span data-ttu-id="e6d28-249">*Dodawanie kontrolera MVC z szkieletem*</span><span class="sxs-lookup"><span data-stu-id="e6d28-249">*Adding an MVC controller with scaffolding*</span></span>
4. <span data-ttu-id="e6d28-250">W obszarze **Klasa kontekstu danych**kliknij pozycję **nowy kontekst danych..** .</span><span class="sxs-lookup"><span data-stu-id="e6d28-250">Under **Data context class**, click **New data context...**.</span></span>

    ![Tworzenie nowego kontekstu danych](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image16.png)

    <span data-ttu-id="e6d28-252">*Tworzenie nowego kontekstu danych*</span><span class="sxs-lookup"><span data-stu-id="e6d28-252">*Creating a new data context*</span></span>
5. <span data-ttu-id="e6d28-253">W oknie dialogowym **nowy kontekst danych** Nazwij nowy kontekst danych *PersonContext* i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="e6d28-253">In the **New Data Context** dialog box, name the new data context *PersonContext* and click **Add**.</span></span>

    ![Tworzenie nowego PersonContext](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image17.png)

    <span data-ttu-id="e6d28-255">*Tworzenie nowego typu PersonContext*</span><span class="sxs-lookup"><span data-stu-id="e6d28-255">*Creating the new PersonContext type*</span></span>
6. <span data-ttu-id="e6d28-256">Kliknij przycisk **Dodaj** , aby utworzyć nowy kontroler dla **osoby** z szkieletem.</span><span class="sxs-lookup"><span data-stu-id="e6d28-256">Click **Add** to create the new controller for **Person** with scaffolding.</span></span> <span data-ttu-id="e6d28-257">Program Visual Studio wygeneruje następnie akcje kontrolera, kontekst danych osoby i widoki Razor.</span><span class="sxs-lookup"><span data-stu-id="e6d28-257">Visual Studio will then generate the controller actions, the Person data context and the Razor views.</span></span>

    ![Po utworzeniu kontrolera MVC przy użyciu szkieletu](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image18.png)

    <span data-ttu-id="e6d28-259">*Po utworzeniu kontrolera MVC przy użyciu szkieletu*</span><span class="sxs-lookup"><span data-stu-id="e6d28-259">*After creating the MVC controller with scaffolding*</span></span>
7. <span data-ttu-id="e6d28-260">Otwórz plik **MvcPersonController.cs** w folderze **controllers** .</span><span class="sxs-lookup"><span data-stu-id="e6d28-260">Open the **MvcPersonController.cs** file in the **Controllers** folder.</span></span> <span data-ttu-id="e6d28-261">Zauważ, że metody akcji CRUD zostały wygenerowane automatycznie.</span><span class="sxs-lookup"><span data-stu-id="e6d28-261">Notice that the CRUD action methods have been generated automatically.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample4.cs)]

    > [!NOTE]
    > <span data-ttu-id="e6d28-262">Zaznaczając pole wyboru **Użyj akcji kontrolera asynchronicznego** z opcji tworzenia szkieletów w poprzednich krokach, program Visual Studio generuje asynchroniczne metody akcji dla wszystkich akcji, które obejmują dostęp do kontekstu danych osoby.</span><span class="sxs-lookup"><span data-stu-id="e6d28-262">By selecting the **Use async controller actions** check box from the scaffolding options in the previous steps, Visual Studio generates asynchronous action methods for all actions that involve access to the Person data context.</span></span> <span data-ttu-id="e6d28-263">Zalecane jest używanie asynchronicznych metod akcji dla długotrwałych żądań niezwiązanych z procesorem CPU, aby uniknąć zablokowania pracy serwera sieci Web podczas przetwarzania żądania.</span><span class="sxs-lookup"><span data-stu-id="e6d28-263">It is recommended that you use asynchronous action methods for long-running, non-CPU bound requests to avoid blocking the Web server from performing work while the request is being processed.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3--running-the-solution"></a><span data-ttu-id="e6d28-264">Zadanie 3 — Uruchamianie rozwiązania</span><span class="sxs-lookup"><span data-stu-id="e6d28-264">Task 3 – Running the Solution</span></span>

<span data-ttu-id="e6d28-265">W tym zadaniu zostanie ponownie uruchomione rozwiązanie, aby sprawdzić, czy widoki dla **osób** działają zgodnie z oczekiwaniami.</span><span class="sxs-lookup"><span data-stu-id="e6d28-265">In this task, you will run the solution again to verify that the views for **Person** are working as expected.</span></span> <span data-ttu-id="e6d28-266">Dodasz nową osobę, aby sprawdzić, czy została pomyślnie zapisana w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="e6d28-266">You will add a new person to verify that it is successfully saved to the database.</span></span>

1. <span data-ttu-id="e6d28-267">Naciśnij klawisz **F5** , aby uruchomić rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="e6d28-267">Press **F5** to run the solution.</span></span>
2. <span data-ttu-id="e6d28-268">Przejdź do **/MvcPerson**.</span><span class="sxs-lookup"><span data-stu-id="e6d28-268">Navigate to **/MvcPerson**.</span></span> <span data-ttu-id="e6d28-269">Widok szkieletu, który pokazuje listę osób, powinna zostać wyświetlona.</span><span class="sxs-lookup"><span data-stu-id="e6d28-269">The scaffolded view that shows the list of people should appear.</span></span>
3. <span data-ttu-id="e6d28-270">Kliknij przycisk **Utwórz nowy** , aby dodać nową osobę.</span><span class="sxs-lookup"><span data-stu-id="e6d28-270">Click **Create New** to add a new person.</span></span>

    ![Przechodzenie do widoków szkieletowych MVC](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image19.png)

    <span data-ttu-id="e6d28-272">*Przechodzenie do widoków szkieletowych MVC*</span><span class="sxs-lookup"><span data-stu-id="e6d28-272">*Navigating to the scaffolded MVC views*</span></span>
4. <span data-ttu-id="e6d28-273">W widoku **Utwórz** Podaj **nazwę** i **wiek** osoby, a następnie kliknij pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="e6d28-273">In the **Create** view, provide a **Name** and an **Age** for the person, and click **Create**.</span></span>

    ![Dodawanie nowej osoby](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image20.png)

    <span data-ttu-id="e6d28-275">*Dodawanie nowej osoby*</span><span class="sxs-lookup"><span data-stu-id="e6d28-275">*Adding a new person*</span></span>
5. <span data-ttu-id="e6d28-276">Nowa osoba zostanie dodana do listy.</span><span class="sxs-lookup"><span data-stu-id="e6d28-276">The new person is added to the list.</span></span> <span data-ttu-id="e6d28-277">Na liście element kliknij pozycję **szczegóły** , aby wyświetlić widok szczegółów osoby.</span><span class="sxs-lookup"><span data-stu-id="e6d28-277">In the element list, click **Details** to display the person's details view.</span></span> <span data-ttu-id="e6d28-278">Następnie w widoku **szczegółów** kliknij przycisk **wstecz do listy** , aby wrócić do widoku listy.</span><span class="sxs-lookup"><span data-stu-id="e6d28-278">Then, in the **Details** view, click **Back to List** to go back to the list view.</span></span>

    ![Widok szczegółów osoby](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image21.png)

    <span data-ttu-id="e6d28-280">*Widok szczegółów osoby*</span><span class="sxs-lookup"><span data-stu-id="e6d28-280">*Person's details view*</span></span>
6. <span data-ttu-id="e6d28-281">Kliknij link **Usuń** , aby usunąć osobę.</span><span class="sxs-lookup"><span data-stu-id="e6d28-281">Click the **Delete** link to delete the person.</span></span> <span data-ttu-id="e6d28-282">W widoku **Usuń** kliknij pozycję **Usuń** , aby potwierdzić operację.</span><span class="sxs-lookup"><span data-stu-id="e6d28-282">In the **Delete** view, click **Delete** to confirm the operation.</span></span>

    ![Usuwanie osoby](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image22.png)

    <span data-ttu-id="e6d28-284">*Usuwanie osoby*</span><span class="sxs-lookup"><span data-stu-id="e6d28-284">*Deleting a person*</span></span>
7. <span data-ttu-id="e6d28-285">Wróć do programu Visual Studio i naciśnij klawisze **Shift + F5** , aby zatrzymać debugowanie.</span><span class="sxs-lookup"><span data-stu-id="e6d28-285">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>

<a id="Exercise3"></a>
### <a name="exercise-3-creating-a-web-api-controller-using-scaffolding"></a><span data-ttu-id="e6d28-286">Ćwiczenie 3: Tworzenie kontrolera interfejsu API sieci Web za pomocą szkieletu</span><span class="sxs-lookup"><span data-stu-id="e6d28-286">Exercise 3: Creating a Web API Controller Using Scaffolding</span></span>

<span data-ttu-id="e6d28-287">Struktura internetowego interfejsu API jest częścią stosu ASP.NET i została zaprojektowana w celu ułatwienia wdrażania usług HTTP, zazwyczaj wysyłania i otrzymywania danych w formacie JSON lub XML za pośrednictwem interfejsu API RESTful.</span><span class="sxs-lookup"><span data-stu-id="e6d28-287">The Web API framework is part of the ASP.NET Stack and designed to make implementing HTTP services easier, generally sending and receiving JSON- or XML-formatted data through a RESTful API.</span></span>

<span data-ttu-id="e6d28-288">W tym ćwiczeniu ponownie użyjesz szkieletu ASP.NET, aby wygenerować kontroler interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="e6d28-288">In this exercise, you will use ASP.NET Scaffolding again to generate a Web API controller.</span></span> <span data-ttu-id="e6d28-289">Użyjesz tej samej **osoby** i **PersonContext** z poprzedniego ćwiczenia, aby zapewnić te same dane osobom w formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="e6d28-289">You will use the same **Person** and **PersonContext** classes from the previous exercise to provide the same person data in JSON format.</span></span> <span data-ttu-id="e6d28-290">Zobaczysz, jak można uwidocznić te same zasoby na różne sposoby w tej samej aplikacji ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e6d28-290">You will see how you can expose the same resources in different ways within the same ASP.NET application.</span></span>

<a id="Ex3Task1"></a>
#### <a name="task-1--creating-a-web-api-controller"></a><span data-ttu-id="e6d28-291">Zadanie 1 — Tworzenie kontrolera interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="e6d28-291">Task 1 – Creating a Web API Controller</span></span>

<span data-ttu-id="e6d28-292">W tym zadaniu utworzysz nowy **kontroler interfejsu API sieci Web** , który będzie uwidaczniał dane osoby w formacie, który jest przeznaczony do użycia maszynowego, na przykład JSON.</span><span class="sxs-lookup"><span data-stu-id="e6d28-292">In this task you will create a new **Web API Controller** that will expose the person data in a machine-consumable format like JSON.</span></span>

1. <span data-ttu-id="e6d28-293">Jeśli nie zostało to jeszcze zrobione, Otwórz **Visual Studio Express 2013 dla sieci Web** i Otwórz rozwiązanie **MyHybridSite. sln** znajdujące się w folderze **Source/Ex3-WebAPI/BEGIN** .</span><span class="sxs-lookup"><span data-stu-id="e6d28-293">If not already opened, open **Visual Studio Express 2013 for Web** and open the **MyHybridSite.sln** solution located in the **Source/Ex3-WebAPI/Begin** folder.</span></span> <span data-ttu-id="e6d28-294">Alternatywnie możesz kontynuować z rozwiązaniem uzyskanym w poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="e6d28-294">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e6d28-295">Jeśli zaczniesz od rozwiązania BEGIN 3, naciśnij **kombinację klawiszy Ctrl + Shift + B** , aby skompilować rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="e6d28-295">If you start with the Begin solution from Exercise 3, press **CTRL + SHIFT + B** to build the solution.</span></span>
2. <span data-ttu-id="e6d28-296">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder **controllers (kontrolery** ) projektu **MyHybridSite** i wybierz polecenie **Dodaj | Nowy element szkieletowy..** .</span><span class="sxs-lookup"><span data-stu-id="e6d28-296">In **Solution Explorer**, right-click the **Controllers** folder of the **MyHybridSite** project and select **Add | New Scaffolded Item...**.</span></span>

    ![Tworzenie nowego kontrolera szkieletowego](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image23.png)

    <span data-ttu-id="e6d28-298">*Tworzenie nowego kontrolera szkieletowego*</span><span class="sxs-lookup"><span data-stu-id="e6d28-298">*Creating a new scaffolded Controller*</span></span>
3. <span data-ttu-id="e6d28-299">W oknie dialogowym **Dodawanie szkieletu** wybierz pozycję **interfejs API sieci Web** w okienku po lewej stronie, a następnie kliknij pozycję **Kontroler sieci Web API 2 z akcjami, używając Entity Framework** w środkowym okienku, a następnie klikając przycisk **Dodaj.**</span><span class="sxs-lookup"><span data-stu-id="e6d28-299">In the **Add Scaffold** dialog box, select **Web API** in the left pane, then **Web API 2 Controller with actions, using Entity Framework** in the middle pane and then click **Add.**</span></span>

    <span data-ttu-id="e6d28-300">![Wybieranie kontrolera Web API 2 z akcjami i Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "Wybieranie kontrolera Web API 2 z akcjami i Entity Framework")</span><span class="sxs-lookup"><span data-stu-id="e6d28-300">![Selecting Web API 2 Controller with actions and Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "Selecting Web API 2 Controller with actions and Entity Framework")</span></span>

    <span data-ttu-id="e6d28-301">*Wybieranie kontrolera Web API 2 z akcjami i Entity Framework*</span><span class="sxs-lookup"><span data-stu-id="e6d28-301">*Selecting Web API 2 Controller with actions and Entity Framework*</span></span>
4. <span data-ttu-id="e6d28-302">Ustaw *ApiPersonController* jako **nazwę kontrolera**, wybierz opcję **Użyj akcji kontrolerów asynchronicznych** i wybierz **osobę (MyHybridSite. models)** i **PersonContext (MyHybridSite. models)** jako odpowiednio **model** i klasy **kontekstu danych** .</span><span class="sxs-lookup"><span data-stu-id="e6d28-302">Set *ApiPersonController* as the **Controller name**, select the **Use async controller actions** option and select **Person (MyHybridSite.Models)** and **PersonContext (MyHybridSite.Models)** as the **Model** and **Data context** classes respectively.</span></span> <span data-ttu-id="e6d28-303">Następnie kliknij pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="e6d28-303">Then click **Add**.</span></span>

    <span data-ttu-id="e6d28-304">![Dodawanie kontrolera interfejsu API sieci Web z szkieletem](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "Dodawanie kontrolera interfejsu API sieci Web z szkieletem")</span><span class="sxs-lookup"><span data-stu-id="e6d28-304">![Adding a Web API Controller with scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "Adding a Web API controller with scaffolding")</span></span>

    <span data-ttu-id="e6d28-305">*Dodawanie kontrolera interfejsu API sieci Web z szkieletem*</span><span class="sxs-lookup"><span data-stu-id="e6d28-305">*Adding a Web API controller with scaffolding*</span></span>
5. <span data-ttu-id="e6d28-306">Program Visual Studio wygeneruje następnie klasę **ApiPersonController** z czterema akcjami CRUD do pracy z danymi.</span><span class="sxs-lookup"><span data-stu-id="e6d28-306">Visual Studio will then generate the **ApiPersonController** class with the four CRUD actions to work with your data.</span></span>

    <span data-ttu-id="e6d28-307">![Po utworzeniu kontrolera interfejsu API sieci Web przy użyciu szkieletu](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "Po utworzeniu kontrolera interfejsu API sieci Web przy użyciu szkieletu")</span><span class="sxs-lookup"><span data-stu-id="e6d28-307">![After creating the Web API controller with scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "After creating the Web API controller with scaffolding")</span></span>

    <span data-ttu-id="e6d28-308">*Po utworzeniu kontrolera interfejsu API sieci Web przy użyciu szkieletu*</span><span class="sxs-lookup"><span data-stu-id="e6d28-308">*After creating the Web API controller with scaffolding*</span></span>
6. <span data-ttu-id="e6d28-309">Otwórz plik **ApiPersonController.cs** i sprawdź *metodę akcji* GetActions.</span><span class="sxs-lookup"><span data-stu-id="e6d28-309">Open the **ApiPersonController.cs** file and inspect the *GetPeople* action method.</span></span> <span data-ttu-id="e6d28-310">Ta metoda wysyła zapytanie do pola DB typu **PersonContext** w celu uzyskania danych dotyczących osób.</span><span class="sxs-lookup"><span data-stu-id="e6d28-310">This method queries the db field of **PersonContext** type in order to get the people data.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample5.cs)]
7. <span data-ttu-id="e6d28-311">Teraz Zwróć uwagę na komentarz powyżej definicji metody.</span><span class="sxs-lookup"><span data-stu-id="e6d28-311">Now notice the comment above the method definition.</span></span> <span data-ttu-id="e6d28-312">Zawiera identyfikator URI, który uwidacznia tę akcję, która zostanie użyta w następnym zadaniu.</span><span class="sxs-lookup"><span data-stu-id="e6d28-312">It provides the URI that exposes this action which you will use in the next task.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample6.cs)]

    > [!NOTE]
    > <span data-ttu-id="e6d28-313">Domyślnie internetowy interfejs API jest skonfigurowany do przechwytywania zapytań do ścieżki */API* , aby uniknąć kolizji z kontrolerami MVC.</span><span class="sxs-lookup"><span data-stu-id="e6d28-313">By default, Web API is configured to catch the queries to the */api* path to avoid collisions with MVC controllers.</span></span> <span data-ttu-id="e6d28-314">Jeśli chcesz zmienić to ustawienie, zapoznaj się z tematem [Routing in ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="e6d28-314">If you need to change this setting, refer to [Routing in ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

<a id="Ex3Task2"></a>
#### <a name="task-2--running-the-solution"></a><span data-ttu-id="e6d28-315">Zadanie 2 — Uruchamianie rozwiązania</span><span class="sxs-lookup"><span data-stu-id="e6d28-315">Task 2 – Running the Solution</span></span>

<span data-ttu-id="e6d28-316">W tym zadaniu będziesz używać **narzędzi deweloperskich** programu Internet Explorer F12, aby sprawdzić pełną odpowiedź z kontrolera interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="e6d28-316">In this task you will use the Internet Explorer **F12 developer tools** to inspect the full response from the Web API controller.</span></span> <span data-ttu-id="e6d28-317">Zobaczysz, jak można przechwytywać ruch sieciowy, aby uzyskać więcej informacji o danych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e6d28-317">You will see how you can capture network traffic to get more insight into your application data.</span></span>

> [!NOTE]
> <span data-ttu-id="e6d28-318">Upewnij **się, że** na pasku narzędzi programu Visual Studio jest wybrany program **Internet Explorer** .</span><span class="sxs-lookup"><span data-stu-id="e6d28-318">Make sure that **Internet Explorer** is selected in the **Start** button located on the Visual Studio toolbar.</span></span>
> 
> ![Opcja programu Internet Explorer](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image27.png)
> 
> <span data-ttu-id="e6d28-320">**Narzędzia programistyczne F12** mają szeroki zestaw funkcji, które nie są omówione w tym ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="e6d28-320">The **F12 developer tools** have a wide set of functionality that is not covered in this hands-on-lab.</span></span> <span data-ttu-id="e6d28-321">Jeśli chcesz dowiedzieć się więcej na ten temat, zobacz [Korzystanie z narzędzi programistycznych F12](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).</span><span class="sxs-lookup"><span data-stu-id="e6d28-321">If you want to learn more about it, refer to [Using the F12 developer tools](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).</span></span>

1. <span data-ttu-id="e6d28-322">Naciśnij klawisz **F5** , aby uruchomić rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="e6d28-322">Press **F5** to run the solution.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e6d28-323">Aby prawidłowo wykonać to zadanie, aplikacja musi mieć dane.</span><span class="sxs-lookup"><span data-stu-id="e6d28-323">In order to follow this task correctly, your application needs to have data.</span></span> <span data-ttu-id="e6d28-324">Jeśli baza danych jest pusta, możesz wrócić do zadania 3 w ćwiczeniu 2 i postępować zgodnie z instrukcjami dotyczącymi tworzenia nowej osoby przy użyciu widoków MVC.</span><span class="sxs-lookup"><span data-stu-id="e6d28-324">If your database is empty, you can go back to Task 3 in Exercise 2 and follow the steps on how to create a new person using the MVC views.</span></span>
2. <span data-ttu-id="e6d28-325">W przeglądarce naciśnij klawisz **F12** , aby otworzyć panel **Narzędzia deweloperskie** .</span><span class="sxs-lookup"><span data-stu-id="e6d28-325">In the browser, press **F12** to open the **Developer Tools** panel.</span></span> <span data-ttu-id="e6d28-326">Naciśnij klawisz **CTRL** + **4** lub kliknij ikonę **sieci** , a następnie kliknij przycisk Zielona strzałka, aby rozpocząć przechwytywanie ruchu sieciowego.</span><span class="sxs-lookup"><span data-stu-id="e6d28-326">Press **CTRL** + **4** or click the **Network** icon, and then click the green arrow button to begin capturing network traffic.</span></span>

    <span data-ttu-id="e6d28-327">![Inicjowanie przechwytywania sieci Web API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "Inicjowanie przechwytywania sieci Web API")</span><span class="sxs-lookup"><span data-stu-id="e6d28-327">![Initiating Web API network capture](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "Initiating Web API network capture")</span></span>

    <span data-ttu-id="e6d28-328">*Inicjowanie przechwytywania sieci Web API*</span><span class="sxs-lookup"><span data-stu-id="e6d28-328">*Initiating Web API network capture*</span></span>
3. <span data-ttu-id="e6d28-329">Dołącz **interfejs API/ApiPerson** do adresu URL na pasku adresu przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="e6d28-329">Append **api/ApiPerson** to the URL in the browser's address bar.</span></span> <span data-ttu-id="e6d28-330">Teraz sprawdzisz szczegóły odpowiedzi z **ApiPersonController**.</span><span class="sxs-lookup"><span data-stu-id="e6d28-330">You will now inspect the details of the response from the **ApiPersonController**.</span></span>

    <span data-ttu-id="e6d28-331">![Pobieranie danych osoby za poorednictwem internetowego interfejsu API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "Pobieranie danych osoby za poorednictwem internetowego interfejsu API")</span><span class="sxs-lookup"><span data-stu-id="e6d28-331">![Retrieving person data through Web API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "Retrieving person data through Web API")</span></span>

    <span data-ttu-id="e6d28-332">*Pobieranie danych osoby za poorednictwem internetowego interfejsu API*</span><span class="sxs-lookup"><span data-stu-id="e6d28-332">*Retrieving person data through Web API*</span></span>

    > [!NOTE]
    > <span data-ttu-id="e6d28-333">Po zakończeniu pobierania zostanie wyświetlony monit o wykonanie akcji z pobranym plikiem.</span><span class="sxs-lookup"><span data-stu-id="e6d28-333">Once the download finishes, you will be prompted to make an action with the downloaded file.</span></span> <span data-ttu-id="e6d28-334">Pozostaw otwarte okno dialogowe, aby móc oglądać zawartość odpowiedzi za pomocą okna narzędzia deweloperów.</span><span class="sxs-lookup"><span data-stu-id="e6d28-334">Leave the dialog box open in order to be able to watch the response content through the Developers Tool window.</span></span>
4. <span data-ttu-id="e6d28-335">Teraz sprawdzimy treść odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="e6d28-335">Now you will inspect the body of the response.</span></span> <span data-ttu-id="e6d28-336">Aby to zrobić, kliknij kartę **szczegóły** , a następnie kliknij pozycję **treść odpowiedzi**.</span><span class="sxs-lookup"><span data-stu-id="e6d28-336">To do this, click the **Details** tab and then click **Response body**.</span></span> <span data-ttu-id="e6d28-337">Możesz sprawdzić, czy pobrane dane to lista obiektów o **identyfikatorze**właściwości, **nazwie** i **wieku** odpowiadającemu klasie **Person** .</span><span class="sxs-lookup"><span data-stu-id="e6d28-337">You can check that the downloaded data is a list of objects with the properties **Id**, **Name** and **Age** that correspond to the **Person** class.</span></span>

    <span data-ttu-id="e6d28-338">![Wyświetlanie treści odpowiedzi interfejsu API sieci Web](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "Wyświetlanie treści odpowiedzi interfejsu API sieci Web")</span><span class="sxs-lookup"><span data-stu-id="e6d28-338">![Viewing Web API Response Body](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "Viewing Web API Response Body")</span></span>

    <span data-ttu-id="e6d28-339">*Wyświetlanie treści odpowiedzi interfejsu API sieci Web*</span><span class="sxs-lookup"><span data-stu-id="e6d28-339">*Viewing Web API Response Body*</span></span>

<a id="Ex3Task3"></a>
#### <a name="task-3--adding-web-api-help-pages"></a><span data-ttu-id="e6d28-340">Zadanie 3 — Dodawanie stron pomocy interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="e6d28-340">Task 3 – Adding Web API Help Pages</span></span>

<span data-ttu-id="e6d28-341">Podczas tworzenia interfejsu API sieci Web warto utworzyć stronę pomocy, aby inni deweloperzy wiedzieli, jak wywołać interfejs API.</span><span class="sxs-lookup"><span data-stu-id="e6d28-341">When you create a Web API, it is useful to create a help page so that other developers will know how to call your API.</span></span> <span data-ttu-id="e6d28-342">Możesz tworzyć i aktualizować strony dokumentacji ręcznie, ale lepiej jest generować je automatycznie, aby uniknąć konieczności wykonywania zadań konserwacyjnych.</span><span class="sxs-lookup"><span data-stu-id="e6d28-342">You could create and update the documentation pages manually, but it is better to auto-generate them to avoid having to do maintenance work.</span></span> <span data-ttu-id="e6d28-343">W tym zadaniu zostanie użyty pakiet NuGet do automatycznego generowania stron pomocy interfejsu API sieci Web w rozwiązaniu.</span><span class="sxs-lookup"><span data-stu-id="e6d28-343">In this task you will use a Nuget package to automatically generate Web API help pages to the solution.</span></span>

1. <span data-ttu-id="e6d28-344">W menu **Narzędzia** w programie Visual Studio wybierz pozycję **Menedżer pakietów NuGet**, a następnie kliknij pozycję **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="e6d28-344">From the **Tools** menu in Visual Studio, select **NuGet Package Manager**, and then click **Package Manager Console**.</span></span>
2. <span data-ttu-id="e6d28-345">W oknie **konsola Menedżera pakietów** wykonaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="e6d28-345">In the **Package Manager Console** window, execute the following command:</span></span>

    [!code-powershell[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample7.ps1)]

    > [!NOTE]
    > <span data-ttu-id="e6d28-346">Pakiet **Microsoft. ASPNET. WebApi. HelpPage** instaluje niezbędne zestawy i dodaje widoki MVC dla stron pomocy w folderze **Areas/HelpPage** .</span><span class="sxs-lookup"><span data-stu-id="e6d28-346">The **Microsoft.AspNet.WebApi.HelpPage** package installs the necessary assemblies and adds MVC Views for the help pages under the **Areas/HelpPage** folder.</span></span>

    <span data-ttu-id="e6d28-347">![Obszar HelpPage](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "Obszar HelpPage")</span><span class="sxs-lookup"><span data-stu-id="e6d28-347">![HelpPage Area](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage Area")</span></span>

    <span data-ttu-id="e6d28-348">*Obszar HelpPage*</span><span class="sxs-lookup"><span data-stu-id="e6d28-348">*HelpPage Area*</span></span>
3. <span data-ttu-id="e6d28-349">Domyślnie strony pomocy mają ciągi zastępcze dla dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="e6d28-349">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="e6d28-350">Możesz użyć komentarzy dokumentacji XML, aby utworzyć dokumentację.</span><span class="sxs-lookup"><span data-stu-id="e6d28-350">You can use XML documentation comments to create the documentation.</span></span> <span data-ttu-id="e6d28-351">Aby włączyć tę funkcję, Otwórz plik **HelpPageConfig.cs** znajdujący się w folderze **Areas/HelpPage/App\_Start** i Usuń komentarz z następującego wiersza:</span><span class="sxs-lookup"><span data-stu-id="e6d28-351">To enable this feature, open the **HelpPageConfig.cs** file located in the **Areas/HelpPage/App\_Start** folder and uncomment the following line:</span></span>

    [!code-javascript[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample8.js)]
4. <span data-ttu-id="e6d28-352">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt **MyHybridSite**, wybierz pozycję **Właściwości** i kliknij kartę **kompilacja** .</span><span class="sxs-lookup"><span data-stu-id="e6d28-352">In **Solution Explorer**, right-click the project **MyHybridSite**, select **Properties** and click the **Build** tab.</span></span>

    <span data-ttu-id="e6d28-353">![Karta kompilacja](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "Sekcja kompilacji")</span><span class="sxs-lookup"><span data-stu-id="e6d28-353">![Build tab](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "Build section")</span></span>

    <span data-ttu-id="e6d28-354">*Karta kompilacja*</span><span class="sxs-lookup"><span data-stu-id="e6d28-354">*Build tab*</span></span>
5. <span data-ttu-id="e6d28-355">W obszarze **dane wyjściowe**wybierz pozycję **plik dokumentacji XML**.</span><span class="sxs-lookup"><span data-stu-id="e6d28-355">Under **Output**, select **XML documentation file**.</span></span> <span data-ttu-id="e6d28-356">W polu edycji wpisz **aplikacja\_dane/XmlDocument. XML**.</span><span class="sxs-lookup"><span data-stu-id="e6d28-356">In the edit box, type **App\_Data/XmlDocument.xml**.</span></span>

    <span data-ttu-id="e6d28-357">![Sekcja danych wyjściowych na karcie kompilacja](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "Sekcja danych wyjściowych na karcie kompilacja")</span><span class="sxs-lookup"><span data-stu-id="e6d28-357">![Output section in Build tab](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "Output section in build tab")</span></span>

    <span data-ttu-id="e6d28-358">*Sekcja danych wyjściowych na karcie kompilacja*</span><span class="sxs-lookup"><span data-stu-id="e6d28-358">*Output section in Build tab*</span></span>
6. <span data-ttu-id="e6d28-359">Naciśnij klawisz **CTRL** + **S** , aby zapisać zmiany.</span><span class="sxs-lookup"><span data-stu-id="e6d28-359">Press **CTRL** + **S** to save the changes.</span></span>
7. <span data-ttu-id="e6d28-360">Otwórz plik **ApiPersonController.cs** z folderu **controllers** .</span><span class="sxs-lookup"><span data-stu-id="e6d28-360">Open the **ApiPersonController.cs** file from the **Controllers** folder.</span></span>
8. <span data-ttu-id="e6d28-361">Wprowadź nowy wiersz między sygnaturą metody *getludzie* i komentarzem *//Get API/ApiPerson* , a następnie wpisz trzy ukośniki.</span><span class="sxs-lookup"><span data-stu-id="e6d28-361">Enter a new line between the *GetPeople* method signature and the *// GET api/ApiPerson* comment, and then type three forward slashes.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e6d28-362">Program Visual Studio automatycznie wstawia elementy XML, które definiują dokumentację metody.</span><span class="sxs-lookup"><span data-stu-id="e6d28-362">Visual Studio automatically inserts the XML elements which define the method documentation.</span></span>
9. <span data-ttu-id="e6d28-363">Dodaj tekst podsumowania i wartość zwracaną dla metody *getludzie* .</span><span class="sxs-lookup"><span data-stu-id="e6d28-363">Add a summary text and the return value for the *GetPeople* method.</span></span> <span data-ttu-id="e6d28-364">Powinien wyglądać podobnie do poniższego.</span><span class="sxs-lookup"><span data-stu-id="e6d28-364">It should look like the following.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample9.cs)]
10. <span data-ttu-id="e6d28-365">Naciśnij klawisz **F5** , aby uruchomić rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="e6d28-365">Press **F5** to run the solution.</span></span>
11. <span data-ttu-id="e6d28-366">Dołącz **/help** do adresu URL na pasku adresu, aby przejść do strony pomocy.</span><span class="sxs-lookup"><span data-stu-id="e6d28-366">Append **/help** to the URL in the address bar to browse to the help page.</span></span>

    <span data-ttu-id="e6d28-367">![Strona pomocy interfejsu API sieci Web ASP.NET](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "Strona pomocy interfejsu API sieci Web ASP.NET")</span><span class="sxs-lookup"><span data-stu-id="e6d28-367">![ASP.NET Web API Help Page](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "ASP.NET Web API Help Page")</span></span>

    <span data-ttu-id="e6d28-368">*Strona pomocy interfejsu API sieci Web ASP.NET*</span><span class="sxs-lookup"><span data-stu-id="e6d28-368">*ASP.NET Web API Help Page*</span></span>

    > [!NOTE]
    > <span data-ttu-id="e6d28-369">Główna Zawartość strony jest tabelą interfejsów API pogrupowanych według kontrolera.</span><span class="sxs-lookup"><span data-stu-id="e6d28-369">The main content of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="e6d28-370">Wpisy tabeli są generowane dynamicznie przy użyciu interfejsu **IApiExplorer** .</span><span class="sxs-lookup"><span data-stu-id="e6d28-370">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="e6d28-371">W przypadku dodania lub zaktualizowania kontrolera interfejsu API tabela zostanie automatycznie zaktualizowana przy następnym kompilowaniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e6d28-371">If you add or update an API controller, the table will be automatically updated the next time you build the application.</span></span>
    > 
    > <span data-ttu-id="e6d28-372">Kolumna **API** zawiera listę metod http i względnego identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="e6d28-372">The **API** column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="e6d28-373">Kolumna **Description** zawiera informacje, które zostały wyodrębnione z dokumentacji metody.</span><span class="sxs-lookup"><span data-stu-id="e6d28-373">The **Description** column contains information that has been extracted from the method's documentation.</span></span>
12. <span data-ttu-id="e6d28-374">Należy zauważyć, że opis dodany powyżej definicji metody jest wyświetlany w kolumnie Opis.</span><span class="sxs-lookup"><span data-stu-id="e6d28-374">Note that the description you added above the method definition is displayed in the description column.</span></span>

    <span data-ttu-id="e6d28-375">![Opis metody interfejsu API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "Opis metody interfejsu API")</span><span class="sxs-lookup"><span data-stu-id="e6d28-375">![API method description](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "API method description")</span></span>

    <span data-ttu-id="e6d28-376">*Opis metody interfejsu API*</span><span class="sxs-lookup"><span data-stu-id="e6d28-376">*API method description*</span></span>
13. <span data-ttu-id="e6d28-377">Kliknij jedną z metod interfejsu API, aby przejść do strony zawierającej bardziej szczegółowe informacje, w tym przykładowe treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="e6d28-377">Click one of the API methods to navigate to a page with more detailed information, including sample response bodies.</span></span>

    <span data-ttu-id="e6d28-378">![Strona informacji szczegółowych](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "Strona informacji szczegółowych")</span><span class="sxs-lookup"><span data-stu-id="e6d28-378">![Detail Information page](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "Detail Information page")</span></span>

    <span data-ttu-id="e6d28-379">*Strona informacji szczegółowych*</span><span class="sxs-lookup"><span data-stu-id="e6d28-379">*Detailed information page*</span></span>

---

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="e6d28-380">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="e6d28-380">Summary</span></span>

<span data-ttu-id="e6d28-381">Dzięki zakończeniu tego praktycznego laboratorium wiesz, jak:</span><span class="sxs-lookup"><span data-stu-id="e6d28-381">By completing this hands-on lab you have learned how to:</span></span>

- <span data-ttu-id="e6d28-382">Utwórz nową aplikację sieci Web przy użyciu jednego środowiska ASP.NET w Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="e6d28-382">Create a new Web application using the One ASP.NET Experience in Visual Studio 2013</span></span>
- <span data-ttu-id="e6d28-383">Integruj wiele technologii ASP.NET w jeden projekt</span><span class="sxs-lookup"><span data-stu-id="e6d28-383">Integrate multiple ASP.NET technologies into one single project</span></span>
- <span data-ttu-id="e6d28-384">Generowanie kontrolerów MVC i widoków z klas modelu za pomocą tworzenia szkieletów ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e6d28-384">Generate MVC controllers and views from your model classes using ASP.NET Scaffolding</span></span>
- <span data-ttu-id="e6d28-385">Generuj Kontrolery interfejsu API sieci Web, które wykorzystują funkcje, takie jak programowanie asynchroniczne i dostęp do danych za pomocą Entity Framework</span><span class="sxs-lookup"><span data-stu-id="e6d28-385">Generate Web API controllers, which use features such as Async Programming and data access through Entity Framework</span></span>
- <span data-ttu-id="e6d28-386">Automatycznie Generuj strony pomocy interfejsu API sieci Web dla Twoich kontrolerów</span><span class="sxs-lookup"><span data-stu-id="e6d28-386">Automatically generate Web API Help Pages for your controllers</span></span>
