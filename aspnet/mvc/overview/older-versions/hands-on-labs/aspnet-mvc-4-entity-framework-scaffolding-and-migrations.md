---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: ASP.NET MVC 4 Entity Framework tworzenia szkieletów i migracji | Microsoft Docs
author: rick-anderson
description: Jeśli znasz metody kontrolera ASP.NET MVC 4 lub zakończył &quot;pomocnicy, formularze i sprawdzanie poprawności&quot; laboratorium, należy pamiętać o tym...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 2b26224390af70e19ca0593abe93a6867140f8ab
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598904"
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a><span data-ttu-id="f415b-103">ASP.NET MVC 4 — tworzenie szkieletu i migracje platformy Entity Framework</span><span class="sxs-lookup"><span data-stu-id="f415b-103">ASP.NET MVC 4 Entity Framework Scaffolding and Migrations</span></span>

<span data-ttu-id="f415b-104">przez [zespół Camp sieci Web](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="f415b-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="f415b-105">Pobierz zestaw szkoleniowy dla sieci Web Camp</span><span class="sxs-lookup"><span data-stu-id="f415b-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="f415b-106">Jeśli znasz metody kontrolera ASP.NET MVC 4 lub zakończono &quot;pomocników, formularzy i walidacji&quot; laboratorium, należy pamiętać, że wiele logiki umożliwia tworzenie, aktualizowanie, wyświetlanie i usuwanie wszystkich jednostek danych, które są powtarzane między aplikacjami.</span><span class="sxs-lookup"><span data-stu-id="f415b-106">If you are familiar with ASP.NET MVC 4 controller methods, or have completed the &quot;Helpers, Forms and Validation&quot; Hands-On lab, you should be aware that many of the logic to create, update, list and remove any data entity it is repeated among the application.</span></span> <span data-ttu-id="f415b-107">Aby nie wspominać, że jeśli model zawiera kilka klas do manipulowania, prawdopodobnie poświęcasz dużo czasu na pisanie metod POST i GET akcji dla każdej operacji jednostki, a także każdego z widoków.</span><span class="sxs-lookup"><span data-stu-id="f415b-107">Not to mention that, if your model has several classes to manipulate, you will be likely to spend a considerable time writing the POST and GET action methods for each entity operation, as well as each of the views.</span></span>

<span data-ttu-id="f415b-108">W tym laboratorium dowiesz się, jak używać szkieletu ASP.NET MVC 4 do automatycznego generowania linii bazowej aplikacji CRUD (tworzenie, odczytywanie, aktualizowanie i usuwanie).</span><span class="sxs-lookup"><span data-stu-id="f415b-108">In this lab you will learn how to use the ASP.NET MVC 4 scaffolding to automatically generate the baseline of your application's CRUD (Create, Read, Update and Delete).</span></span> <span data-ttu-id="f415b-109">Rozpoczynając od prostej klasy modelu i bez pisania pojedynczego wiersza kodu, utworzysz kontroler, który będzie zawierać wszystkie operacje CRUD, a także wszystkie niezbędne widoki.</span><span class="sxs-lookup"><span data-stu-id="f415b-109">Starting from a simple model class, and, without writing a single line of code, you will create a controller that will contain all the CRUD operations, as well as the all the necessary views.</span></span> <span data-ttu-id="f415b-110">Po skompilowaniu i uruchomieniu prostego rozwiązania zostanie wygenerowana baza danych aplikacji wraz z logiką MVC i widokami służącymi do manipulowania danymi.</span><span class="sxs-lookup"><span data-stu-id="f415b-110">After building and running the simple solution, you will have the application database generated, together with the MVC logic and views for data manipulation.</span></span>

<span data-ttu-id="f415b-111">Ponadto dowiesz się, jak łatwo można używać migracji Entity Framework, aby wykonywać aktualizacje modelu w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f415b-111">In addition, you will learn how easy it is to use Entity Framework Migrations to perform model updates throughout your entire application.</span></span> <span data-ttu-id="f415b-112">Entity Framework migracji umożliwią zmodyfikowanie bazy danych po zmianie modelu z prostymi krokami.</span><span class="sxs-lookup"><span data-stu-id="f415b-112">Entity Framework Migrations will let you modify your database after the model has changed with simple steps.</span></span> <span data-ttu-id="f415b-113">Ze względu na to, że będziesz mieć możliwość bardziej wydajnego kompilowania i konserwowania aplikacji sieci Web, korzystając z najnowszych funkcji ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="f415b-113">With all these in mind, you will be able to build and maintain web applications more efficiently, taking advantage of the latest features of ASP.NET MVC 4.</span></span>

> [!NOTE]
> <span data-ttu-id="f415b-114">Wszystkie przykładowe kod i fragmenty kodu są dołączone do zestawu szkoleniowego dla sieci Web Camp, dostępnego w [wersji Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="f415b-114">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="f415b-115">Projekt specyficzny dla tego laboratorium jest dostępny w witrynie [ASP.NET MVC 4 Entity Framework tworzenia szkieletów i migracji](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).</span><span class="sxs-lookup"><span data-stu-id="f415b-115">The project specific to this lab is available at [ASP.NET MVC 4 Entity Framework Scaffolding and Migrations](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="f415b-116">Cele</span><span class="sxs-lookup"><span data-stu-id="f415b-116">Objectives</span></span>

<span data-ttu-id="f415b-117">W tym ćwiczeniu dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="f415b-117">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="f415b-118">Używanie szkieletu ASP.NET dla operacji CRUD na kontrolerach.</span><span class="sxs-lookup"><span data-stu-id="f415b-118">Use ASP.NET scaffolding for CRUD operations in controllers.</span></span>
- <span data-ttu-id="f415b-119">Zmień model bazy danych za pomocą migracji Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f415b-119">Change the database model using Entity Framework Migrations.</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="f415b-120">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="f415b-120">Prerequisites</span></span>

<span data-ttu-id="f415b-121">Aby ukończyć to laboratorium, musisz mieć następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="f415b-121">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="f415b-122">[Microsoft Visual Studio Express 2012 dla sieci Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub nadrzędnych (Przeczytaj [dodatek A,](#AppendixA) Aby uzyskać instrukcje na temat sposobu jego instalacji).</span><span class="sxs-lookup"><span data-stu-id="f415b-122">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="f415b-123">Konfigurowanie</span><span class="sxs-lookup"><span data-stu-id="f415b-123">Setup</span></span>

<span data-ttu-id="f415b-124">**Instalowanie fragmentów kodu**</span><span class="sxs-lookup"><span data-stu-id="f415b-124">**Installing Code Snippets**</span></span>

<span data-ttu-id="f415b-125">Dla wygody większość kodu, który będzie zarządzany w tym laboratorium, jest dostępna jako fragmenty kodu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f415b-125">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="f415b-126">Aby zainstalować plik fragmentów kodu, uruchom **.\Source\Setup\CodeSnippets.VSI** .</span><span class="sxs-lookup"><span data-stu-id="f415b-126">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="f415b-127">Jeśli nie znasz fragmentów kodu Visual Studio Code i chcesz dowiedzieć się, jak z nich korzystać, możesz odwołać się do dodatku z tego dokumentu &quot;[dodatku B: Using fragmenty kodu](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="f415b-127">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="f415b-128">Symulacyjn</span><span class="sxs-lookup"><span data-stu-id="f415b-128">Exercises</span></span>

<span data-ttu-id="f415b-129">Następujące czynności sprawiają, że ćwiczenia te są wykonywane w tym ćwiczeniu:</span><span class="sxs-lookup"><span data-stu-id="f415b-129">The following exercise make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="f415b-130">Używanie szkieletu ASP.NET MVC 4 z migracją Entity Framework</span><span class="sxs-lookup"><span data-stu-id="f415b-130">Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>](#Exercise1)

> [!NOTE]
> <span data-ttu-id="f415b-131">Do tego ćwiczenia dołączany jest folder **końcowy** zawierający rozwiązanie, które należy uzyskać po zakończeniu ćwiczenia.</span><span class="sxs-lookup"><span data-stu-id="f415b-131">This exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercise.</span></span> <span data-ttu-id="f415b-132">Tego rozwiązania można użyć jako przewodnika, jeśli potrzebujesz dodatkowej pomocy w pracy przez ćwiczenie.</span><span class="sxs-lookup"><span data-stu-id="f415b-132">You can use this solution as a guide if you need additional help working through the exercise.</span></span>

<span data-ttu-id="f415b-133">Szacowany czas wykonywania tego laboratorium: **30 minut**</span><span class="sxs-lookup"><span data-stu-id="f415b-133">Estimated time to complete this lab: **30 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a><span data-ttu-id="f415b-134">Ćwiczenie 1: używanie szkieletu ASP.NET MVC 4 z migracją Entity Framework</span><span class="sxs-lookup"><span data-stu-id="f415b-134">Exercise 1: Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>

<span data-ttu-id="f415b-135">Funkcja tworzenia szkieletów ASP.NET MVC umożliwia szybkie generowanie operacji CRUD w ustandaryzowany sposób, co pozwala na współdziałanie aplikacji z warstwą bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f415b-135">ASP.NET MVC scaffolding provides a quick way to generate the CRUD operations in a standardized way, creating the necessary logic that lets your application interact with the database layer.</span></span>

<span data-ttu-id="f415b-136">W tym ćwiczeniu dowiesz się, jak utworzyć metody CRUD przy użyciu szkieletu ASP.NET MVC 4 z kodem.</span><span class="sxs-lookup"><span data-stu-id="f415b-136">In this exercise, you will learn how to use ASP.NET MVC 4 scaffolding with code first to create the CRUD methods.</span></span> <span data-ttu-id="f415b-137">Następnie dowiesz się, jak zaktualizować model, stosując zmiany z bazy danych za pomocą migracji Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f415b-137">Then, you will learn how to update your model applying the changes in the database by using Entity Framework Migrations.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a><span data-ttu-id="f415b-138">Zadanie 1 — Tworzenie nowego projektu ASP.NET MVC 4 przy użyciu szkieletu</span><span class="sxs-lookup"><span data-stu-id="f415b-138">Task 1- Creating a new ASP.NET MVC 4 project using Scaffolding</span></span>

1. <span data-ttu-id="f415b-139">Jeśli nie jest jeszcze otwarty, uruchom **program Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="f415b-139">If not already open, start **Visual Studio 2012**.</span></span>
2. <span data-ttu-id="f415b-140">Wybierz **plik | Nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="f415b-140">Select **File | New Project**.</span></span> <span data-ttu-id="f415b-141">W oknie dialogowym Nowy projekt w obszarze **Wizualizacja C# | Sekcja sieci Web** , wybierz opcję **aplikacja sieci Web ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="f415b-141">In the New Project dialog, under the **Visual C# | Web** section, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="f415b-142">Nadaj projektowi nazwę **MVC4andEFMigrations** i Ustaw lokalizację do folderu **Source\Ex1-UsingMVC4ScaffoldingEFMigrations** w tym laboratorium.</span><span class="sxs-lookup"><span data-stu-id="f415b-142">Name the project to **MVC4andEFMigrations** and set the location to **Source\Ex1-UsingMVC4ScaffoldingEFMigrations** folder of this lab.</span></span> <span data-ttu-id="f415b-143">Ustaw **nazwę rozwiązania** na **Rozpocznij** i upewnij się, że jest zaznaczone pole wyboru **Utwórz katalog dla rozwiązania** .</span><span class="sxs-lookup"><span data-stu-id="f415b-143">Set the **Solution name** to **Begin** and ensure **Create directory for solution** is checked.</span></span> <span data-ttu-id="f415b-144">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="f415b-144">Click **OK**.</span></span>

    <span data-ttu-id="f415b-145">![Okno dialogowe Nowy projekt ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "Okno dialogowe Nowy projekt ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="f415b-145">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="f415b-146">*Okno dialogowe Nowy projekt ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="f415b-146">*New ASP.NET MVC 4 Project Dialog Box*</span></span>
3. <span data-ttu-id="f415b-147">W oknie dialogowym **Nowy projekt ASP.NET MVC 4** wybierz szablon **aplikacja internetowa** i upewnij się, że **Razor** jest wybrany **aparat widoku**.</span><span class="sxs-lookup"><span data-stu-id="f415b-147">In the **New ASP.NET MVC 4 Project** dialog box select the **Internet Application** template, and make sure that **Razor** is the selected **View engine**.</span></span> <span data-ttu-id="f415b-148">Kliknij przycisk **OK**, aby utworzyć projekt.</span><span class="sxs-lookup"><span data-stu-id="f415b-148">Click **OK** to create the project.</span></span>

    <span data-ttu-id="f415b-149">![Nowa aplikacja internetowa ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "Nowa aplikacja internetowa ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="f415b-149">![New ASP.NET MVC 4 Internet Application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "New ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="f415b-150">*Nowa aplikacja internetowa ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="f415b-150">*New ASP.NET MVC 4 Internet Application*</span></span>
4. <span data-ttu-id="f415b-151">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy pozycję **modele** i wybierz polecenie **Dodaj | Klasa** do utworzenia prostej osoby klasy (POCO).</span><span class="sxs-lookup"><span data-stu-id="f415b-151">In the Solution Explorer, right-click **Models** and select **Add | Class** to create a simple class person (POCO).</span></span> <span data-ttu-id="f415b-152">Nadaj nazwę **osobie** IT i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="f415b-152">Name it **Person** and click **OK**.</span></span>
5. <span data-ttu-id="f415b-153">Otwórz klasę Person i Wstaw następujące właściwości.</span><span class="sxs-lookup"><span data-stu-id="f415b-153">Open the Person class and insert the following properties.</span></span>

    <span data-ttu-id="f415b-154">(Fragment kodu — *ASP.NET i migracji Entity Framework MVC 4 — Ex1 właściwości osoby*)</span><span class="sxs-lookup"><span data-stu-id="f415b-154">(Code Snippet - *ASP.NET MVC 4 and Entity Framework Migrations - Ex1 Person Properties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. <span data-ttu-id="f415b-155">Kliknij pozycję **kompilacja | Skompiluj rozwiązanie** , aby zapisać zmiany i skompilować projekt.</span><span class="sxs-lookup"><span data-stu-id="f415b-155">Click **Build | Build Solution** to save the changes and build the project.</span></span>

    <span data-ttu-id="f415b-156">![Kompilowanie aplikacji](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Kompilowanie aplikacji")</span><span class="sxs-lookup"><span data-stu-id="f415b-156">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Building the application")</span></span>

    <span data-ttu-id="f415b-157">*Kompilowanie aplikacji*</span><span class="sxs-lookup"><span data-stu-id="f415b-157">*Building the Application*</span></span>
7. <span data-ttu-id="f415b-158">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy folder controllers, a następnie wybierz pozycję **Dodaj | Kontroler**.</span><span class="sxs-lookup"><span data-stu-id="f415b-158">In the Solution Explorer, right-click the controllers folder and select **Add | Controller**.</span></span>
8. <span data-ttu-id="f415b-159">Nadaj kontrolerowi nazwę *PersonController* i wypełnij **Opcje tworzenia szkieletu** , korzystając z następujących wartości.</span><span class="sxs-lookup"><span data-stu-id="f415b-159">Name the controller *PersonController* and complete the **Scaffolding options** with the following values.</span></span>

   1. <span data-ttu-id="f415b-160">Z listy rozwijanej **szablon** wybierz **kontroler MVC z akcjami odczytu/zapisu i widokami, używając opcji Entity Framework** .</span><span class="sxs-lookup"><span data-stu-id="f415b-160">In the **Template** drop-down list, select the **MVC controller with read/write actions and views, using Entity Framework** option.</span></span>
   2. <span data-ttu-id="f415b-161">Z listy rozwijanej **Klasa modelu** wybierz klasę **Person** .</span><span class="sxs-lookup"><span data-stu-id="f415b-161">In the **Model class** drop-down list, select the **Person** class.</span></span>
   3. <span data-ttu-id="f415b-162">Na liście **Klasa kontekstu danych** wybierz opcję **&lt;nowy kontekst danych...&gt;** .</span><span class="sxs-lookup"><span data-stu-id="f415b-162">In the **Data Context class** list, select **&lt;New data context...&gt;**.</span></span> <span data-ttu-id="f415b-163">Wybierz dowolną nazwę, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="f415b-163">Choose any name and click **OK**.</span></span>
   4. <span data-ttu-id="f415b-164">Upewnij się, że na liście rozwijanej **widoki** została wybrana wartość **Razor** .</span><span class="sxs-lookup"><span data-stu-id="f415b-164">In the **Views** drop-down list, make sure that **Razor** is selected.</span></span>

      <span data-ttu-id="f415b-165">![Dodawanie kontrolera osoby za pomocą szkieletu](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Dodawanie kontrolera osoby za pomocą szkieletu")</span><span class="sxs-lookup"><span data-stu-id="f415b-165">![Adding the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Adding the Person controller with scaffolding")</span></span>

      <span data-ttu-id="f415b-166">*Dodawanie kontrolera osoby za pomocą szkieletu*</span><span class="sxs-lookup"><span data-stu-id="f415b-166">*Adding the Person controller with scaffolding*</span></span>
9. <span data-ttu-id="f415b-167">Kliknij przycisk **Dodaj** , aby utworzyć nowy kontroler dla osoby z szkieletem.</span><span class="sxs-lookup"><span data-stu-id="f415b-167">Click **Add** to create the new controller for Person with scaffolding.</span></span> <span data-ttu-id="f415b-168">Teraz zostały wygenerowane akcje kontrolera oraz widoki.</span><span class="sxs-lookup"><span data-stu-id="f415b-168">You have now generated the controller actions as well as the views.</span></span>

    <span data-ttu-id="f415b-169">![Po utworzeniu kontrolera osoby przy użyciu szkieletu](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "Po utworzeniu kontrolera osoby przy użyciu szkieletu")</span><span class="sxs-lookup"><span data-stu-id="f415b-169">![After creating the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "After creating the Person controller with scaffolding")</span></span>

    <span data-ttu-id="f415b-170">*Po utworzeniu kontrolera osoby przy użyciu szkieletu*</span><span class="sxs-lookup"><span data-stu-id="f415b-170">*After creating the Person controller with scaffolding*</span></span>
10. <span data-ttu-id="f415b-171">Otwórz klasę **PersonController** .</span><span class="sxs-lookup"><span data-stu-id="f415b-171">Open **PersonController** class.</span></span> <span data-ttu-id="f415b-172">Zauważ, że wszystkie metody akcji CRUD zostały wygenerowane automatycznie.</span><span class="sxs-lookup"><span data-stu-id="f415b-172">Notice that the full CRUD action methods have been generated automatically.</span></span>

   <span data-ttu-id="f415b-173">![Wewnątrz kontrolera osoby](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "Wewnątrz kontrolera osoby")</span><span class="sxs-lookup"><span data-stu-id="f415b-173">![Inside the Person controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "Inside the Person controller")</span></span>

   <span data-ttu-id="f415b-174">*Wewnątrz kontrolera osoby*</span><span class="sxs-lookup"><span data-stu-id="f415b-174">*Inside the Person controller*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a><span data-ttu-id="f415b-175">Zadanie 2 — Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="f415b-175">Task 2- Running the application</span></span>

<span data-ttu-id="f415b-176">W tym momencie baza danych nie została jeszcze utworzona.</span><span class="sxs-lookup"><span data-stu-id="f415b-176">At this point, the database is not yet created.</span></span> <span data-ttu-id="f415b-177">W tym zadaniu uruchomisz aplikację po raz pierwszy i przetestujesz operacje CRUD.</span><span class="sxs-lookup"><span data-stu-id="f415b-177">In this task, you will run the application for the first time and test the CRUD operations.</span></span> <span data-ttu-id="f415b-178">Baza danych zostanie utworzona na bieżąco z Code First.</span><span class="sxs-lookup"><span data-stu-id="f415b-178">The database will be created on the fly with Code First.</span></span>

1. <span data-ttu-id="f415b-179">Naciśnij klawisz **F5**, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="f415b-179">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="f415b-180">W przeglądarce Dodaj **/Person** do adresu URL, aby otworzyć stronę osoby.</span><span class="sxs-lookup"><span data-stu-id="f415b-180">In the browser, add **/Person** to the URL to open the Person page.</span></span>

    <span data-ttu-id="f415b-181">![Aplikacja pierwszego uruchomienia](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "Aplikacja pierwszego uruchomienia")</span><span class="sxs-lookup"><span data-stu-id="f415b-181">![Application first run](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "Application first run")</span></span>

    <span data-ttu-id="f415b-182">*Aplikacja: pierwsze uruchomienie*</span><span class="sxs-lookup"><span data-stu-id="f415b-182">*Application: first run*</span></span>
3. <span data-ttu-id="f415b-183">Zobaczysz teraz strony osoby i przetestujesz operacje CRUD.</span><span class="sxs-lookup"><span data-stu-id="f415b-183">You will now explore the Person pages and test the CRUD operations.</span></span>

    1. <span data-ttu-id="f415b-184">Kliknij przycisk **Utwórz nowy** , aby dodać nową osobę.</span><span class="sxs-lookup"><span data-stu-id="f415b-184">Click **Create New** to add a new person.</span></span> <span data-ttu-id="f415b-185">Wprowadź imię i nazwisko, a następnie kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="f415b-185">Enter a first name and a last name and click **Create**.</span></span>

        <span data-ttu-id="f415b-186">![Dodawanie nowej osoby](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Dodawanie nowej osoby")</span><span class="sxs-lookup"><span data-stu-id="f415b-186">![Adding a new person](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Adding a new person")</span></span>

        <span data-ttu-id="f415b-187">*Dodawanie nowej osoby*</span><span class="sxs-lookup"><span data-stu-id="f415b-187">*Adding a new person*</span></span>
    2. <span data-ttu-id="f415b-188">Na liście osób można usuwać, edytować lub dodawać elementy.</span><span class="sxs-lookup"><span data-stu-id="f415b-188">In the person's list, you can delete, edit or add items.</span></span>

        <span data-ttu-id="f415b-189">![Lista osób](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "Lista osób")</span><span class="sxs-lookup"><span data-stu-id="f415b-189">![person list](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "person list")</span></span>

        <span data-ttu-id="f415b-190">*Lista osób*</span><span class="sxs-lookup"><span data-stu-id="f415b-190">*Person list*</span></span>
    3. <span data-ttu-id="f415b-191">Kliknij przycisk **szczegóły** , aby otworzyć Szczegóły osoby.</span><span class="sxs-lookup"><span data-stu-id="f415b-191">Click **Details** to open the person's details.</span></span>

        <span data-ttu-id="f415b-192">![Szczegóły osoby](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "Szczegóły osoby")</span><span class="sxs-lookup"><span data-stu-id="f415b-192">![Person's details](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "Person's details")</span></span>

        <span data-ttu-id="f415b-193">*Szczegóły osoby*</span><span class="sxs-lookup"><span data-stu-id="f415b-193">*Person's details*</span></span>
4. <span data-ttu-id="f415b-194">Zamknij przeglądarkę i wróć do programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f415b-194">Close the browser and return to Visual Studio.</span></span> <span data-ttu-id="f415b-195">Zwróć uwagę, że utworzono całą CRUD dla podmiotu osoby w całej aplikacji — od modelu do widoków — bez konieczności pisania pojedynczego wiersza kodu.</span><span class="sxs-lookup"><span data-stu-id="f415b-195">Notice that you have created the whole CRUD for the person entity throughout your application -from the model to the views- without having to write a single line of code!</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a><span data-ttu-id="f415b-196">Zadanie 3 — Aktualizowanie bazy danych za pomocą migracji Entity Framework</span><span class="sxs-lookup"><span data-stu-id="f415b-196">Task 3- Updating the database using Entity Framework Migrations</span></span>

<span data-ttu-id="f415b-197">W tym zadaniu zostanie zaktualizowana baza danych przy użyciu Entity Framework migracji.</span><span class="sxs-lookup"><span data-stu-id="f415b-197">In this task you will update the database using Entity Framework Migrations.</span></span> <span data-ttu-id="f415b-198">Zobaczysz, jak łatwo można zmienić model i odzwierciedlić zmiany w bazach danych za pomocą funkcji migracji Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f415b-198">You will discover how easy it is to change the model and reflect the changes in your databases by using the Entity Framework Migrations feature.</span></span>

1. <span data-ttu-id="f415b-199">Otwórz konsolę Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="f415b-199">Open the Package Manager Console.</span></span> <span data-ttu-id="f415b-200">Wybierz kolejno pozycje **narzędzia** > **menedżer pakietów NuGet** > **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="f415b-200">Select **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>
2. <span data-ttu-id="f415b-201">W konsoli Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="f415b-201">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="f415b-202">PMC</span><span class="sxs-lookup"><span data-stu-id="f415b-202">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    <span data-ttu-id="f415b-203">![Włączanie migracji](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Włączanie migracji")</span><span class="sxs-lookup"><span data-stu-id="f415b-203">![Enabling Migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Enabling Migrations")</span></span>

    <span data-ttu-id="f415b-204">*Włączanie migracji*</span><span class="sxs-lookup"><span data-stu-id="f415b-204">*Enabling migrations*</span></span>

    <span data-ttu-id="f415b-205">Polecenie Enable-Migration tworzy folder **migracji** , który zawiera skrypt umożliwiający zainicjowanie bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f415b-205">The Enable-Migration command creates the **Migrations** folder, which contains a script to initialize the database.</span></span>

    <span data-ttu-id="f415b-206">![Folder migracji](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Folder migracji")</span><span class="sxs-lookup"><span data-stu-id="f415b-206">![Migrations folder](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Migrations folder")</span></span>

    <span data-ttu-id="f415b-207">*Folder migracji*</span><span class="sxs-lookup"><span data-stu-id="f415b-207">*Migrations folder*</span></span>
3. <span data-ttu-id="f415b-208">Otwórz plik **Configuration.cs** w folderze migrations.</span><span class="sxs-lookup"><span data-stu-id="f415b-208">Open the **Configuration.cs** file in the Migrations folder.</span></span> <span data-ttu-id="f415b-209">Znajdź konstruktora klasy i zmień wartość **AutomaticMigrationsEnabled** na *true*.</span><span class="sxs-lookup"><span data-stu-id="f415b-209">Locate the class constructor and change the **AutomaticMigrationsEnabled** value to *true*.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. <span data-ttu-id="f415b-210">Otwórz klasę Person i Dodaj atrybut do nazwy środkowej osoby.</span><span class="sxs-lookup"><span data-stu-id="f415b-210">Open the Person class and add an attribute for the person's middle name.</span></span> <span data-ttu-id="f415b-211">Ten nowy atrybut zmienia model.</span><span class="sxs-lookup"><span data-stu-id="f415b-211">With this new attribute, you are changing the model.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. <span data-ttu-id="f415b-212">Wybierz **kompilację | Kompiluj rozwiązanie** w menu, aby skompilować aplikację.</span><span class="sxs-lookup"><span data-stu-id="f415b-212">Select **Build | Build Solution** on the menu to build the application.</span></span>

    <span data-ttu-id="f415b-213">![Kompilowanie aplikacji](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Kompilowanie aplikacji")</span><span class="sxs-lookup"><span data-stu-id="f415b-213">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Building the application")</span></span>

    <span data-ttu-id="f415b-214">*Kompilowanie aplikacji*</span><span class="sxs-lookup"><span data-stu-id="f415b-214">*Building the application*</span></span>
6. <span data-ttu-id="f415b-215">W konsoli Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="f415b-215">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="f415b-216">PMC</span><span class="sxs-lookup"><span data-stu-id="f415b-216">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    <span data-ttu-id="f415b-217">To polecenie spowoduje przeszukanie zmian w obiektach danych, a następnie doda odpowiednie polecenia, aby odpowiednio zmodyfikować bazę danych.</span><span class="sxs-lookup"><span data-stu-id="f415b-217">This command will look for changes in the data objects, and then, it will add the necessary commands to modify the database accordingly.</span></span>

    <span data-ttu-id="f415b-218">![Dodawanie nazwy środkowej](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Dodawanie nazwy środkowej")</span><span class="sxs-lookup"><span data-stu-id="f415b-218">![Adding a middle name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Adding a middle name")</span></span>

    <span data-ttu-id="f415b-219">*Dodawanie nazwy środkowej*</span><span class="sxs-lookup"><span data-stu-id="f415b-219">*Adding a middle name*</span></span>
7. <span data-ttu-id="f415b-220">Obowiązkowe Można uruchomić następujące polecenie, aby wygenerować skrypt SQL z aktualizacją różnicową.</span><span class="sxs-lookup"><span data-stu-id="f415b-220">(Optional) You can run the following command to generate a SQL script with the differential update.</span></span> <span data-ttu-id="f415b-221">Pozwoli to na aktualizację bazy danych ręcznie (w tym przypadku nie jest to konieczne) lub zastosowanie zmian w innych bazach danych:</span><span class="sxs-lookup"><span data-stu-id="f415b-221">This will let you update the database manually (In this case it's not necessary), or apply the changes in other databases:</span></span>

    <span data-ttu-id="f415b-222">PMC</span><span class="sxs-lookup"><span data-stu-id="f415b-222">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    <span data-ttu-id="f415b-223">![Generowanie skryptu SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Generowanie skryptu SQL")</span><span class="sxs-lookup"><span data-stu-id="f415b-223">![Generating a SQL script](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Generating a SQL script")</span></span>

    <span data-ttu-id="f415b-224">*Generowanie skryptu SQL*</span><span class="sxs-lookup"><span data-stu-id="f415b-224">*Generating a SQL script*</span></span>

    <span data-ttu-id="f415b-225">![Aktualizacja skryptu SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "Aktualizacja skryptu SQL")</span><span class="sxs-lookup"><span data-stu-id="f415b-225">![SQL Script update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "SQL Script update")</span></span>

    <span data-ttu-id="f415b-226">*Aktualizacja skryptu SQL*</span><span class="sxs-lookup"><span data-stu-id="f415b-226">*SQL Script update*</span></span>
8. <span data-ttu-id="f415b-227">W konsoli Menedżera pakietów wprowadź następujące polecenie, aby zaktualizować bazę danych:</span><span class="sxs-lookup"><span data-stu-id="f415b-227">In the Package Manager Console, enter the following command to update the database:</span></span>

    <span data-ttu-id="f415b-228">PMC</span><span class="sxs-lookup"><span data-stu-id="f415b-228">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    <span data-ttu-id="f415b-229">![Aktualizowanie bazy danych](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "aktualizowanie bazy danych")</span><span class="sxs-lookup"><span data-stu-id="f415b-229">![Updating the Database](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "Updating the Database")</span></span>

    <span data-ttu-id="f415b-230">*Aktualizowanie bazy danych*</span><span class="sxs-lookup"><span data-stu-id="f415b-230">*Updating the Database*</span></span>

    <span data-ttu-id="f415b-231">Spowoduje to dodanie kolumny **MiddleName** w tabeli **osób** w celu dopasowania jej do bieżącej definicji klasy **Person** .</span><span class="sxs-lookup"><span data-stu-id="f415b-231">This will add the **MiddleName** column in the **People** table to match the current definition of the **Person** class.</span></span>
9. <span data-ttu-id="f415b-232">Gdy baza danych zostanie zaktualizowana, kliknij prawym przyciskiem myszy folder kontroler i wybierz polecenie **Dodaj | Kontroler** , aby ponownie dodać kontrolera osoby (wykonaj te same wartości).</span><span class="sxs-lookup"><span data-stu-id="f415b-232">Once the database is updated, right-click the Controller folder and select **Add | Controller** to add the Person controller again (Complete with the same values).</span></span> <span data-ttu-id="f415b-233">Spowoduje to zaktualizowanie istniejących metod i widoków dodając nowy atrybut.</span><span class="sxs-lookup"><span data-stu-id="f415b-233">This will update the existing methods and views adding the new attribute.</span></span>

    <span data-ttu-id="f415b-234">![Dodawanie aktualizacji kontrolera](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Dodawanie aktualizacji kontrolera")</span><span class="sxs-lookup"><span data-stu-id="f415b-234">![Adding a controller update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Adding a controller update")</span></span>

    <span data-ttu-id="f415b-235">*Aktualizowanie kontrolera*</span><span class="sxs-lookup"><span data-stu-id="f415b-235">*Updating the controller*</span></span>
10. <span data-ttu-id="f415b-236">Kliknij pozycję **Add** (Dodaj).</span><span class="sxs-lookup"><span data-stu-id="f415b-236">Click **Add**.</span></span> <span data-ttu-id="f415b-237">Następnie wybierz wartości **Zastąp PersonController.cs** i **Zastąp powiązane widoki** , a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="f415b-237">Then, select the values **Overwrite PersonController.cs** and the **Overwrite associated views** and click **OK**.</span></span>

   ![Dodawanie zastąpienia kontrolera](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   <span data-ttu-id="f415b-239">*Aktualizowanie kontrolera*</span><span class="sxs-lookup"><span data-stu-id="f415b-239">*Updating the controller*</span></span>

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a><span data-ttu-id="f415b-240">Task4 — uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="f415b-240">Task4- Running the application</span></span>

1. <span data-ttu-id="f415b-241">Naciśnij klawisz **F5**, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="f415b-241">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="f415b-242">Otwórz **/Person**.</span><span class="sxs-lookup"><span data-stu-id="f415b-242">Open **/Person**.</span></span> <span data-ttu-id="f415b-243">Zwróć uwagę, że dane zostały zachowane, podczas gdy została dodana kolumna nazwy środkowej.</span><span class="sxs-lookup"><span data-stu-id="f415b-243">Notice that the data was preserved, while the middle name column was added.</span></span>

    <span data-ttu-id="f415b-244">![Dodano środkową nazwę](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Dodano środkową nazwę")</span><span class="sxs-lookup"><span data-stu-id="f415b-244">![Middle Name added](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Middle Name added")</span></span>

    <span data-ttu-id="f415b-245">*Dodano środkową nazwę*</span><span class="sxs-lookup"><span data-stu-id="f415b-245">*Middle Name added*</span></span>
3. <span data-ttu-id="f415b-246">Jeśli klikniesz pozycję **Edytuj**, będziesz mieć możliwość dodania nazwy Środkowej do bieżącej osoby.</span><span class="sxs-lookup"><span data-stu-id="f415b-246">If you click **Edit**, you will be able to add a middle name to the current person.</span></span>

    <span data-ttu-id="f415b-247">![Wersja środkowa](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "Wersja środkowa")</span><span class="sxs-lookup"><span data-stu-id="f415b-247">![Middle Name edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "Middle Name edition")</span></span>

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="f415b-248">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="f415b-248">Summary</span></span>

<span data-ttu-id="f415b-249">W tym ćwiczeniu dowiesz się, jak tworzyć operacje CRUD z użyciem szkieletu ASP.NET MVC 4 przy użyciu dowolnej klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="f415b-249">In this Hands-On lab, you have learned simple steps to create CRUD operations with ASP.NET MVC 4 Scaffolding using any model class.</span></span> <span data-ttu-id="f415b-250">Następnie wiesz już, jak wykonać aktualizację kompleksową w aplikacji — z bazy danych do widoków — za pomocą migracji Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f415b-250">Then, you have learned how to perform an end to end update in your application -from the database to the views- by using Entity Framework Migrations.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="f415b-251">Dodatek A: Instalowanie Visual Studio Express 2012 dla sieci Web</span><span class="sxs-lookup"><span data-stu-id="f415b-251">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="f415b-252">Można zainstalować **Microsoft Visual Studio Express 2012 dla sieci Web** lub innej &quot;Express&quot; wersji za pomocą **[Instalator platformy Microsoft Web](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="f415b-252">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="f415b-253">Poniższe instrukcje przeprowadzą Cię przez kroki wymagane do zainstalowania *programu Visual Studio Express 2012 for Web* przy użyciu *Instalator platformy Microsoft Web*.</span><span class="sxs-lookup"><span data-stu-id="f415b-253">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="f415b-254">Przejdź do obszaru [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169) (Ustawienia — Integracje i usługi).</span><span class="sxs-lookup"><span data-stu-id="f415b-254">Go to [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="f415b-255">Alternatywnie, jeśli zainstalowano już Instalatora platformy sieci Web, można go otworzyć i wyszukać produkt &quot;<em>Visual Studio Express 2012 for Web przy użyciu zestawu Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="f415b-255">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="f415b-256">Kliknij pozycję **Zainstaluj teraz**.</span><span class="sxs-lookup"><span data-stu-id="f415b-256">Click on **Install Now**.</span></span> <span data-ttu-id="f415b-257">Jeśli nie masz **Instalatora platformy sieci Web** , nastąpi przekierowanie do pobrania i zainstalowania go w pierwszej kolejności.</span><span class="sxs-lookup"><span data-stu-id="f415b-257">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="f415b-258">Po otwarciu **Instalatora platformy sieci Web** kliknij przycisk **Zainstaluj** , aby rozpocząć instalację.</span><span class="sxs-lookup"><span data-stu-id="f415b-258">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="f415b-259">![Zainstaluj Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Zainstaluj Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="f415b-259">![Install Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="f415b-260">*Zainstaluj Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="f415b-260">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="f415b-261">Przeczytaj wszystkie licencje i postanowienia produktów, a następnie kliknij przycisk **Akceptuję** , aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="f415b-261">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akceptowanie postanowień licencyjnych](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    <span data-ttu-id="f415b-263">*Akceptowanie postanowień licencyjnych*</span><span class="sxs-lookup"><span data-stu-id="f415b-263">*Accepting the license terms*</span></span>
5. <span data-ttu-id="f415b-264">Poczekaj na zakończenie procesu pobierania i instalacji.</span><span class="sxs-lookup"><span data-stu-id="f415b-264">Wait until the downloading and installation process completes.</span></span>

    ![Postęp instalacji](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    <span data-ttu-id="f415b-266">*Postęp instalacji*</span><span class="sxs-lookup"><span data-stu-id="f415b-266">*Installation progress*</span></span>
6. <span data-ttu-id="f415b-267">Po zakończeniu instalacji kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="f415b-267">When the installation completes, click **Finish**.</span></span>

    ![Instalacja zakończona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    <span data-ttu-id="f415b-269">*Instalacja zakończona*</span><span class="sxs-lookup"><span data-stu-id="f415b-269">*Installation completed*</span></span>
7. <span data-ttu-id="f415b-270">Kliknij przycisk **Zakończ** , aby zamknąć Instalatora platformy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="f415b-270">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="f415b-271">Aby otworzyć Visual Studio Express dla sieci Web, przejdź do ekranu **startowego** i zacznij pisać &quot;**vs Express**&quot;, a następnie kliknij kafelek **vs Express for Web** .</span><span class="sxs-lookup"><span data-stu-id="f415b-271">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Kafelek VS Express for Web](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    <span data-ttu-id="f415b-273">*Kafelek VS Express for Web*</span><span class="sxs-lookup"><span data-stu-id="f415b-273">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="f415b-274">Dodatek B: używanie fragmentów kodu</span><span class="sxs-lookup"><span data-stu-id="f415b-274">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="f415b-275">Za pomocą fragmentów kodu masz cały kod, którego potrzebujesz na wyręką.</span><span class="sxs-lookup"><span data-stu-id="f415b-275">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="f415b-276">Dokument laboratoryjny powiedzie się dokładnie wtedy, gdy można z nich korzystać, jak pokazano na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="f415b-276">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="f415b-277">![Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio")</span><span class="sxs-lookup"><span data-stu-id="f415b-277">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="f415b-278">*Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="f415b-278">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="f415b-279">***Aby dodać fragment kodu przy użyciu klawiatury (C# tylko)***</span><span class="sxs-lookup"><span data-stu-id="f415b-279">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="f415b-280">Umieść kursor, w którym chcesz wstawić kod.</span><span class="sxs-lookup"><span data-stu-id="f415b-280">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="f415b-281">Zacznij wpisywać nazwę fragmentu kodu (bez spacji lub łączników).</span><span class="sxs-lookup"><span data-stu-id="f415b-281">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="f415b-282">Obejrzyj jako IntelliSense wyświetla pasujące nazwy fragmentów kodu.</span><span class="sxs-lookup"><span data-stu-id="f415b-282">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="f415b-283">Wybierz poprawny fragment kodu (lub Kontynuuj wpisywanie, dopóki nie zostanie zaznaczona cała nazwa fragmentu kodu).</span><span class="sxs-lookup"><span data-stu-id="f415b-283">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="f415b-284">Naciśnij dwukrotnie klawisz Tab, aby wstawić fragment kodu w lokalizacji kursora.</span><span class="sxs-lookup"><span data-stu-id="f415b-284">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="f415b-285">![Zacznij wpisywać nazwę fragmentu kodu](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "Zacznij wpisywać nazwę fragmentu kodu")</span><span class="sxs-lookup"><span data-stu-id="f415b-285">![Start typing the snippet name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="f415b-286">*Zacznij wpisywać nazwę fragmentu kodu*</span><span class="sxs-lookup"><span data-stu-id="f415b-286">*Start typing the snippet name*</span></span>

<span data-ttu-id="f415b-287">![Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu")</span><span class="sxs-lookup"><span data-stu-id="f415b-287">![Press Tab to select the highlighted snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="f415b-288">*Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="f415b-288">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="f415b-289">![Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta")</span><span class="sxs-lookup"><span data-stu-id="f415b-289">![Press Tab again and the snippet will expand](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="f415b-290">*Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta*</span><span class="sxs-lookup"><span data-stu-id="f415b-290">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="f415b-291">***Aby dodać fragment kodu przy użyciu myszy (C#, Visual Basic i XML)*** jedno.</span><span class="sxs-lookup"><span data-stu-id="f415b-291">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="f415b-292">Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu.</span><span class="sxs-lookup"><span data-stu-id="f415b-292">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="f415b-293">Wybierz **Wstaw fragment** kodu, po którym następuje **Moje fragmenty kodów**.</span><span class="sxs-lookup"><span data-stu-id="f415b-293">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="f415b-294">Wybierz odpowiedni fragment kodu z listy, klikając go.</span><span class="sxs-lookup"><span data-stu-id="f415b-294">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="f415b-295">![Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment")</span><span class="sxs-lookup"><span data-stu-id="f415b-295">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="f415b-296">*Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment*</span><span class="sxs-lookup"><span data-stu-id="f415b-296">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="f415b-297">![Wybierz odpowiedni fragment kodu z listy, klikając go](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "Wybierz odpowiedni fragment kodu z listy, klikając go")</span><span class="sxs-lookup"><span data-stu-id="f415b-297">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="f415b-298">*Wybierz odpowiedni fragment kodu z listy, klikając go*</span><span class="sxs-lookup"><span data-stu-id="f415b-298">*Pick the relevant snippet from the list, by clicking on it*</span></span>
