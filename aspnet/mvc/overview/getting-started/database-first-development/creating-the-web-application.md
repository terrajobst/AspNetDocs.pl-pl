---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 'Samouczek: Tworzenie aplikacji sieci Web i modeli danych dla EF Database First za pomocą ASP.NET MVC'
description: Ten samouczek koncentruje się na tworzeniu aplikacji sieci Web i generowaniu modeli danych opartych na tabelach bazy danych.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: 30fd42be5677df6fa6ee0630914098c30d21385b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616271"
---
# <a name="tutorial-create-the-web-application-and-data-models-for-ef-database-first-with-aspnet-mvc"></a><span data-ttu-id="4cfd3-103">Samouczek: Tworzenie aplikacji sieci Web i modeli danych dla EF Database First za pomocą ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="4cfd3-103">Tutorial: Create the Web Application and Data Models for EF Database First with ASP.NET MVC</span></span>

 <span data-ttu-id="4cfd3-104">Korzystając z szkieletów MVC, Entity Framework i ASP.NET, można utworzyć aplikację sieci Web, która udostępnia interfejs istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="4cfd3-105">W tej serii samouczków pokazano, jak automatycznie generować kod, który umożliwia użytkownikom wyświetlanie, edytowanie, tworzenie i usuwanie danych znajdujących się w tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="4cfd3-106">Wygenerowany kod odpowiada kolumnom w tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="4cfd3-107">Ten samouczek koncentruje się na tworzeniu aplikacji sieci Web i generowaniu modeli danych opartych na tabelach bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-107">This tutorial focuses on creating the web application, and generating the data models based on your database tables.</span></span>

<span data-ttu-id="4cfd3-108">W tym samouczku zostaną wykonane następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="4cfd3-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4cfd3-109">Tworzenie aplikacji internetowej platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4cfd3-109">Create an ASP.NET web app</span></span>
> * <span data-ttu-id="4cfd3-110">Generuj modele</span><span class="sxs-lookup"><span data-stu-id="4cfd3-110">Generate the models</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4cfd3-111">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="4cfd3-111">Prerequisites</span></span>

* [<span data-ttu-id="4cfd3-112">Wprowadzenie do Entity Framework 6 Database First przy użyciu MVC 5</span><span class="sxs-lookup"><span data-stu-id="4cfd3-112">Getting started with Entity Framework 6 Database First using MVC 5</span></span>](setting-up-database.md)

## <a name="create-an-aspnet-web-app"></a><span data-ttu-id="4cfd3-113">Tworzenie aplikacji internetowej platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4cfd3-113">Create an ASP.NET web app</span></span>

<span data-ttu-id="4cfd3-114">W nowym rozwiązaniu lub w tym samym rozwiązaniu co projekt bazy danych Utwórz nowy projekt w programie Visual Studio i wybierz szablon **aplikacji sieci Web ASP.NET** .</span><span class="sxs-lookup"><span data-stu-id="4cfd3-114">In either a new solution or the same solution as the database project, create a new project in Visual Studio and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="4cfd3-115">Nazwij projekt **ContosoSite**.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-115">Name the project **ContosoSite**.</span></span>

![Utwórz projekt](creating-the-web-application/_static/image1.png)

<span data-ttu-id="4cfd3-117">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-117">Click **OK**.</span></span>

<span data-ttu-id="4cfd3-118">W oknie Nowy projekt ASP.NET wybierz szablon **MVC** .</span><span class="sxs-lookup"><span data-stu-id="4cfd3-118">In the New ASP.NET Project window, select the **MVC** template.</span></span> <span data-ttu-id="4cfd3-119">Możesz teraz wyczyścić opcję **host w chmurze** , ponieważ później zostanie wdrożona aplikacja w chmurze.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-119">You can clear the **Host in the cloud** option for now because you will deploy the application to the cloud later.</span></span> <span data-ttu-id="4cfd3-120">Kliknij przycisk **OK** , aby utworzyć aplikację.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-120">Click **OK** to create the application.</span></span>

<span data-ttu-id="4cfd3-121">Projekt jest tworzony z domyślnymi plikami i folderami.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-121">The project is created with the default files and folders.</span></span>

<span data-ttu-id="4cfd3-122">W tym samouczku użyjesz Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-122">In this tutorial, you will use Entity Framework 6.</span></span> <span data-ttu-id="4cfd3-123">Możesz dwukrotnie sprawdzić wersję Entity Framework w projekcie za pomocą okna Zarządzanie pakietami NuGet.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-123">You can double-check the version of Entity Framework in your project through the Manage NuGet Packages window.</span></span> <span data-ttu-id="4cfd3-124">W razie potrzeby Zaktualizuj swoją wersję Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-124">If necessary, update your version of Entity Framework.</span></span>

![Pokaż wersję](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a><span data-ttu-id="4cfd3-126">Generuj modele</span><span class="sxs-lookup"><span data-stu-id="4cfd3-126">Generate the models</span></span>

<span data-ttu-id="4cfd3-127">Teraz utworzysz modele Entity Framework z tabel bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-127">You will now create Entity Framework models from the database tables.</span></span> <span data-ttu-id="4cfd3-128">Te modele są klasami, które będą używane do pracy z danymi.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-128">These models are classes that you will use to work with the data.</span></span> <span data-ttu-id="4cfd3-129">Każdy model odzwierciedla tabelę w bazie danych i zawiera właściwości, które odnoszą się do kolumn w tabeli.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-129">Each model mirrors a table in the database and contains properties that correspond to the columns in the table.</span></span>

<span data-ttu-id="4cfd3-130">Kliknij prawym przyciskiem myszy folder **modele** , a następnie wybierz polecenie **Dodaj** i **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-130">Right-click the **Models** folder, and select **Add** and **New Item**.</span></span>

<span data-ttu-id="4cfd3-131">W oknie Dodawanie nowego elementu wybierz pozycję **dane** w lewym okienku i **ADO.NET Entity Data Model** z opcji w środkowym okienku.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-131">In the Add New Item window, select **Data** in the left pane and **ADO.NET Entity Data Model** from the options in the center pane.</span></span> <span data-ttu-id="4cfd3-132">Nazwij nowy plik modelu **ContosoModel**.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-132">Name the new model file **ContosoModel**.</span></span>

<span data-ttu-id="4cfd3-133">Kliknij pozycję **Add** (Dodaj).</span><span class="sxs-lookup"><span data-stu-id="4cfd3-133">Click **Add**.</span></span>

<span data-ttu-id="4cfd3-134">W Kreatorze Entity Data Model wybierz pozycję **Dr Designer z bazy danych**.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-134">In the Entity Data Model Wizard, select **EF Designer from database**.</span></span>

<span data-ttu-id="4cfd3-135">Kliknij przycisk **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-135">Click **Next**.</span></span>

<span data-ttu-id="4cfd3-136">Jeśli masz połączenia bazy danych zdefiniowane w środowisku deweloperskim, może zostać wyświetlony jeden z tych połączeń wstępnie wybranych.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-136">If you have database connections defined within your development environment, you may see one of these connections pre-selected.</span></span> <span data-ttu-id="4cfd3-137">Jednak chcesz utworzyć nowe połączenie z bazą danych utworzoną w pierwszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-137">However, you want to create a new connection to the database you created in the first part of this tutorial.</span></span> <span data-ttu-id="4cfd3-138">Kliknij przycisk **nowe połączenie** .</span><span class="sxs-lookup"><span data-stu-id="4cfd3-138">Click the **New Connection** button.</span></span>

<span data-ttu-id="4cfd3-139">W okno Właściwości połączenia Podaj nazwę lokalnego serwera, na którym została utworzona baza danych (w tym przypadku **(LocalDB) \ProjectsV13**).</span><span class="sxs-lookup"><span data-stu-id="4cfd3-139">In the Connection Properties window, provide the name of the local server where your database was created (in this case **(localdb)\ProjectsV13**).</span></span> <span data-ttu-id="4cfd3-140">Po podania nazwy serwera wybierz ContosoUniversityData z dostępnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-140">After providing the server name, select the ContosoUniversityData from the available databases.</span></span>

![Ustawianie właściwości połączenia](creating-the-web-application/_static/image8.png)

<span data-ttu-id="4cfd3-142">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-142">Click **OK**.</span></span>

<span data-ttu-id="4cfd3-143">Zostaną wyświetlone poprawne właściwości połączenia.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-143">The correct connection properties are now displayed.</span></span> <span data-ttu-id="4cfd3-144">Możesz użyć nazwy domyślnej dla połączenia w pliku Web. config.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-144">You can use the default name for connection in the Web.Config file.</span></span>

<span data-ttu-id="4cfd3-145">Kliknij przycisk **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-145">Click **Next**.</span></span>

<span data-ttu-id="4cfd3-146">Wybierz najnowszą wersję Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-146">Select the latest version of Entity Framework.</span></span>

<span data-ttu-id="4cfd3-147">Kliknij przycisk **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-147">Click **Next**.</span></span>

<span data-ttu-id="4cfd3-148">Wybierz pozycję **tabele** , aby wygenerować modele dla wszystkich trzech tabel.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-148">Select **Tables** to generate models for all three tables.</span></span>

<span data-ttu-id="4cfd3-149">Kliknij przycisk **Finish** (Zakończ).</span><span class="sxs-lookup"><span data-stu-id="4cfd3-149">Click **Finish**.</span></span>

<span data-ttu-id="4cfd3-150">Jeśli zostanie wyświetlone ostrzeżenie o zabezpieczeniach, wybierz **przycisk OK** , aby kontynuować uruchamianie szablonu.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-150">If you receive a security warning, select **OK** to continue running the template.</span></span>

<span data-ttu-id="4cfd3-151">Modele są generowane na podstawie tabel bazy danych i wyświetlany jest diagram, który pokazuje właściwości i relacje między tabelami.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-151">The models are generated from the database tables, and a diagram is displayed that shows the properties and relationships between the tables.</span></span>

![Diagram modelu](creating-the-web-application/_static/image11.png)

<span data-ttu-id="4cfd3-153">Folder modele zawiera teraz wiele nowych plików związanych z modelami, które zostały wygenerowane na podstawie bazy danych programu.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-153">The Models folder now includes many new files related to the models that were generated from the database.</span></span>

<span data-ttu-id="4cfd3-154">Plik **ContosoModel.Context.cs** zawiera klasę, która dziedziczy z klasy **DbContext** , i udostępnia właściwość dla każdej klasy modelu, która odpowiada tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-154">The **ContosoModel.Context.cs** file contains a class that derives from the **DbContext** class, and provides a property for each model class that corresponds to a database table.</span></span> <span data-ttu-id="4cfd3-155">Pliki **Course.cs**, **Enrollment.cs**i **student.cs** zawierają klasy modelu, które reprezentują tabele baz danych.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-155">The **Course.cs**, **Enrollment.cs**, and **Student.cs** files contain the model classes that represent the databases tables.</span></span> <span data-ttu-id="4cfd3-156">Podczas pracy z szkieletami należy używać zarówno klasy kontekstu, jak i klas modelu.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-156">You will use both the context class and the model classes when working with scaffolding.</span></span>

<span data-ttu-id="4cfd3-157">Przed kontynuowaniem pracy z tym samouczkiem Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-157">Before proceeding with this tutorial, build the project.</span></span> <span data-ttu-id="4cfd3-158">W następnej sekcji zostanie wygenerowany kod oparty na modelach danych, ale ta sekcja nie będzie działała, jeśli projekt nie został skompilowany.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-158">In the next section, you will generate code based on the data models, but that section will not work if the project has not been built.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4cfd3-159">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="4cfd3-159">Next steps</span></span>

<span data-ttu-id="4cfd3-160">W tym samouczku zostaną wykonane następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="4cfd3-160">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4cfd3-161">Utworzono aplikację sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4cfd3-161">Created an ASP.NET web app</span></span>
> * <span data-ttu-id="4cfd3-162">Wygenerowane modele</span><span class="sxs-lookup"><span data-stu-id="4cfd3-162">Generated the models</span></span>

<span data-ttu-id="4cfd3-163">Przejdź do następnego samouczka, aby dowiedzieć się, jak utworzyć generowanie kodu na podstawie modeli danych.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-163">Advance to the next tutorial to learn how to create generate code based on the data models.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="4cfd3-164">Generowanie widoków</span><span class="sxs-lookup"><span data-stu-id="4cfd3-164">Generating views</span></span>](generating-views.md)
