---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: ASP.NET pomocników MVC 4, formularze i sprawdzanie poprawności | Microsoft Docs
author: rick-anderson
description: W przypadku modeli ASP.NET MVC 4 i dostępu do danych w laboratorium załadowane i wyświetlane są dane z bazy danych. W tym ćwiczeniu dowiesz się, jak dodać do...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: 0e2605a4188eaf814f6ab0ebfeaabed4457bcfa3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78539579"
---
# <a name="aspnet-mvc-4-helpers-forms-and-validation"></a><span data-ttu-id="03e4a-104">ASP.NET MVC 4 — pomocnicy, formularze i walidacja</span><span class="sxs-lookup"><span data-stu-id="03e4a-104">ASP.NET MVC 4 Helpers, Forms and Validation</span></span>

<span data-ttu-id="03e4a-105">przez [zespół Camp sieci Web](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="03e4a-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="03e4a-106">Pobierz zestaw szkoleniowy dla sieci Web Camp</span><span class="sxs-lookup"><span data-stu-id="03e4a-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="03e4a-107">W przypadku **modeli ASP.NET MVC 4 i dostępu do danych** w laboratorium załadowane i wyświetlane są dane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="03e4a-107">In **ASP.NET MVC 4 Models and Data Access** Hands-on Lab, you have been loading and displaying data from the database.</span></span> <span data-ttu-id="03e4a-108">W tym ćwiczeniu należy dodać do aplikacji ze **sklepu muzycznego** możliwość edytowania tych danych.</span><span class="sxs-lookup"><span data-stu-id="03e4a-108">In this Hands-on Lab, you will add to the **Music Store** application the ability to edit that data.</span></span>

<span data-ttu-id="03e4a-109">W tym celu należy najpierw utworzyć kontroler, który będzie obsługiwał akcje tworzenia, odczytu, aktualizacji i usuwania (CRUD) albumów.</span><span class="sxs-lookup"><span data-stu-id="03e4a-109">With that goal in mind, you will first create the controller that will support the Create, Read, Update and Delete (CRUD) actions of albums.</span></span> <span data-ttu-id="03e4a-110">Zostanie wygenerowany szablon widoku indeksu wykorzystujący funkcję tworzenia szkieletów ASP.NET MVC, aby wyświetlić właściwości albumów w tabeli HTML.</span><span class="sxs-lookup"><span data-stu-id="03e4a-110">You will generate an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span> <span data-ttu-id="03e4a-111">Aby ulepszyć ten widok, należy dodać niestandardowego pomocnika HTML, który będzie obcinał długi opis.</span><span class="sxs-lookup"><span data-stu-id="03e4a-111">To enhance that view, you will add a custom HTML helper that will truncate long descriptions.</span></span>

<span data-ttu-id="03e4a-112">Następnie dodasz widoki Edytuj i Utwórz, które umożliwią zmianę albumów w bazie danych, z pomocą elementów formularza, takich jak listy rozwijane.</span><span class="sxs-lookup"><span data-stu-id="03e4a-112">Afterwards, you will add the Edit and Create Views that will let you alter the albums in the database, with the help of form elements like dropdowns.</span></span>

<span data-ttu-id="03e4a-113">Na koniec zezwolisz użytkownikom na usunięcie albumu, a ponadto uniemożliwimy im wprowadzanie nieprawidłowych danych, sprawdzając ich dane wejściowe.</span><span class="sxs-lookup"><span data-stu-id="03e4a-113">Lastly, you will let users delete an album and also you will prevent them from entering wrong data by validating their input.</span></span>

<span data-ttu-id="03e4a-114">W ramach tego ćwiczenia praktycznego założono, że masz podstawową wiedzę na temat **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-114">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="03e4a-115">Jeśli wcześniej nie korzystasz z **ASP.NET MVC** , zalecamy przejście do środowiska ASP.NET w oparciu o podstawowe informacje dotyczące **MVC** .</span><span class="sxs-lookup"><span data-stu-id="03e4a-115">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC Fundamentals** Hands-on Lab.</span></span>

<span data-ttu-id="03e4a-116">To laboratorium przeprowadzi Cię przez ulepszenia i nowe funkcje opisane wcześniej przez zastosowanie drobnych zmian do przykładowej aplikacji sieci Web, która znajduje się w folderze źródłowym.</span><span class="sxs-lookup"><span data-stu-id="03e4a-116">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>

> [!NOTE]
> <span data-ttu-id="03e4a-117">Wszystkie przykładowe kod i fragmenty kodu są zawarte w zestawie szkoleń w sieci Web Camp, które są dostępne w [wersjach Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="03e4a-117">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="03e4a-118">Projekt specyficzny dla tego laboratorium jest dostępny w [ASP.NETach, formularzach i weryfikacji w MVC 4](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).</span><span class="sxs-lookup"><span data-stu-id="03e4a-118">The project specific to this lab is available at [ASP.NET MVC 4 Helpers, Forms and Validation](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="03e4a-119">Cele</span><span class="sxs-lookup"><span data-stu-id="03e4a-119">Objectives</span></span>

<span data-ttu-id="03e4a-120">W tym ćwiczeniu dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="03e4a-120">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="03e4a-121">Tworzenie kontrolera do obsługi operacji CRUD</span><span class="sxs-lookup"><span data-stu-id="03e4a-121">Create a controller to support CRUD operations</span></span>
- <span data-ttu-id="03e4a-122">Generuj widok indeksu, aby wyświetlić właściwości jednostki w tabeli HTML</span><span class="sxs-lookup"><span data-stu-id="03e4a-122">Generate an Index View to display entity properties in an HTML table</span></span>
- <span data-ttu-id="03e4a-123">Dodawanie niestandardowego pomocnika HTML</span><span class="sxs-lookup"><span data-stu-id="03e4a-123">Add a custom HTML helper</span></span>
- <span data-ttu-id="03e4a-124">Tworzenie i Dostosowywanie widoku edycji</span><span class="sxs-lookup"><span data-stu-id="03e4a-124">Create and customize an Edit View</span></span>
- <span data-ttu-id="03e4a-125">Różnice między metodami akcji, które reagują na wywołania HTTP-GET lub HTTP-POST</span><span class="sxs-lookup"><span data-stu-id="03e4a-125">Differentiate between action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="03e4a-126">Dodawanie i Dostosowywanie widoku tworzenia</span><span class="sxs-lookup"><span data-stu-id="03e4a-126">Add and customize a Create View</span></span>
- <span data-ttu-id="03e4a-127">Obsłuż Usuwanie jednostki</span><span class="sxs-lookup"><span data-stu-id="03e4a-127">Handle the deletion of an entity</span></span>
- <span data-ttu-id="03e4a-128">Weryfikowanie danych wejściowych użytkownika</span><span class="sxs-lookup"><span data-stu-id="03e4a-128">Validate user input</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="03e4a-129">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="03e4a-129">Prerequisites</span></span>

<span data-ttu-id="03e4a-130">Aby ukończyć to laboratorium, musisz mieć następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="03e4a-130">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="03e4a-131">[Microsoft Visual Studio Express 2012 dla sieci Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub nadrzędnych (Przeczytaj [dodatek A,](#AppendixA) Aby uzyskać instrukcje na temat sposobu jego instalacji).</span><span class="sxs-lookup"><span data-stu-id="03e4a-131">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="03e4a-132">Konfigurowanie</span><span class="sxs-lookup"><span data-stu-id="03e4a-132">Setup</span></span>

<span data-ttu-id="03e4a-133">**Instalowanie fragmentów kodu**</span><span class="sxs-lookup"><span data-stu-id="03e4a-133">**Installing Code Snippets**</span></span>

<span data-ttu-id="03e4a-134">Dla wygody większość kodu, który będzie zarządzany w tym laboratorium, jest dostępna jako fragmenty kodu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="03e4a-134">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="03e4a-135">Aby zainstalować plik fragmentów kodu, uruchom **.\Source\Setup\CodeSnippets.VSI** .</span><span class="sxs-lookup"><span data-stu-id="03e4a-135">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="03e4a-136">Jeśli nie znasz fragmentów kodu Visual Studio Code i chcesz dowiedzieć się, jak z nich korzystać, możesz odwołać się do dodatku z tego dokumentu &quot;[dodatku B: Using fragmenty kodu](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="03e4a-136">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="03e4a-137">Symulacyjn</span><span class="sxs-lookup"><span data-stu-id="03e4a-137">Exercises</span></span>

<span data-ttu-id="03e4a-138">Następujące ćwiczenia składają się na te praktyczne laboratorium:</span><span class="sxs-lookup"><span data-stu-id="03e4a-138">The following exercises make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="03e4a-139">Tworzenie kontrolera Menedżera sklepu i jego widoku indeksu</span><span class="sxs-lookup"><span data-stu-id="03e4a-139">Creating the Store Manager controller and its Index view</span></span>](#Exercise1)
2. [<span data-ttu-id="03e4a-140">Dodawanie pomocnika HTML</span><span class="sxs-lookup"><span data-stu-id="03e4a-140">Adding an HTML Helper</span></span>](#Exercise2)
3. [<span data-ttu-id="03e4a-141">Tworzenie widoku edycji</span><span class="sxs-lookup"><span data-stu-id="03e4a-141">Creating the Edit View</span></span>](#Exercise3)
4. [<span data-ttu-id="03e4a-142">Dodawanie widoku</span><span class="sxs-lookup"><span data-stu-id="03e4a-142">Adding a Create View</span></span>](#Exercise4)
5. [<span data-ttu-id="03e4a-143">Obsługa usuwania</span><span class="sxs-lookup"><span data-stu-id="03e4a-143">Handling Deletion</span></span>](#Exercise5)
6. [<span data-ttu-id="03e4a-144">Dodawanie weryfikacji</span><span class="sxs-lookup"><span data-stu-id="03e4a-144">Adding Validation</span></span>](#Exercise6)
7. [<span data-ttu-id="03e4a-145">Korzystanie z niezauważalnej technologii jQuery na stronie klienta</span><span class="sxs-lookup"><span data-stu-id="03e4a-145">Using Unobtrusive jQuery at Client Side</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="03e4a-146">Każdemu ćwiczeniu towarzyszy folder **końcowy** zawierający rozwiązanie, które należy uzyskać po zakończeniu ćwiczenia.</span><span class="sxs-lookup"><span data-stu-id="03e4a-146">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="03e4a-147">Tego rozwiązania można użyć jako przewodnika, jeśli potrzebujesz dodatkowej pomocy w pracy przez ćwiczenia.</span><span class="sxs-lookup"><span data-stu-id="03e4a-147">You can use this solution as a guide if you need additional help working through the exercises.</span></span>

<span data-ttu-id="03e4a-148">Szacowany czas wykonywania tego laboratorium: **60 minut**</span><span class="sxs-lookup"><span data-stu-id="03e4a-148">Estimated time to complete this lab: **60 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a><span data-ttu-id="03e4a-149">Ćwiczenie 1: Tworzenie kontrolera Menedżera sklepu i jego widoku indeksu</span><span class="sxs-lookup"><span data-stu-id="03e4a-149">Exercise 1: Creating the Store Manager controller and its Index view</span></span>

<span data-ttu-id="03e4a-150">W tym ćwiczeniu dowiesz się, jak utworzyć nowy kontroler do obsługi operacji CRUD, dostosować jego metodę działania indeksu, aby zwracała listę albumów z bazy danych, a wreszcie wygenerować szablon widoku indeksu wykorzystujący tworzenie szkieletu ASP.NET MVC Funkcja umożliwiająca wyświetlanie właściwości albumów w tabeli HTML.</span><span class="sxs-lookup"><span data-stu-id="03e4a-150">In this exercise, you will learn how to create a new controller to support CRUD operations, customize its Index action method to return a list of albums from the database and finally generating an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a><span data-ttu-id="03e4a-151">Zadanie 1 — Tworzenie StoreManagerController</span><span class="sxs-lookup"><span data-stu-id="03e4a-151">Task 1 - Creating the StoreManagerController</span></span>

<span data-ttu-id="03e4a-152">W tym zadaniu utworzysz nowy kontroler o nazwie **StoreManagerController** do obsługi operacji CRUD.</span><span class="sxs-lookup"><span data-stu-id="03e4a-152">In this task, you will create a new controller called **StoreManagerController** to support CRUD operations.</span></span>

1. <span data-ttu-id="03e4a-153">Otwórz rozwiązanie **BEGIN** znajdujące się w lokalizacji **Source/Ex1-CreatingTheStoreManagerController/BEGIN/** folder.</span><span class="sxs-lookup"><span data-stu-id="03e4a-153">Open the **Begin** solution located at **Source/Ex1-CreatingTheStoreManagerController/Begin/** folder.</span></span>

   1. <span data-ttu-id="03e4a-154">Przed kontynuowaniem musisz pobrać brakujące pakiety NuGet.</span><span class="sxs-lookup"><span data-stu-id="03e4a-154">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="03e4a-155">W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-155">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="03e4a-156">W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.</span><span class="sxs-lookup"><span data-stu-id="03e4a-156">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="03e4a-157">Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-157">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="03e4a-158">Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="03e4a-158">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="03e4a-159">Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu.</span><span class="sxs-lookup"><span data-stu-id="03e4a-159">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="03e4a-160">To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="03e4a-160">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="03e4a-161">Dodaj nowy kontroler.</span><span class="sxs-lookup"><span data-stu-id="03e4a-161">Add a new controller.</span></span> <span data-ttu-id="03e4a-162">Aby to zrobić, kliknij prawym przyciskiem myszy folder **controllers** w obszarze Eksplorator rozwiązań wybierz pozycję **Dodaj** , a następnie polecenie **kontroler** .</span><span class="sxs-lookup"><span data-stu-id="03e4a-162">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="03e4a-163">Zmień nazwę **kontrolera** na **StoreManagerController** i upewnij się, że jest zaznaczona opcja **kontroler MVC z pustymi akcjami odczytu/zapisu** .</span><span class="sxs-lookup"><span data-stu-id="03e4a-163">Change the **Controller** **Name** to **StoreManagerController** and make sure the option **MVC controller with empty read/write actions** is selected.</span></span> <span data-ttu-id="03e4a-164">Kliknij pozycję **Add** (Dodaj).</span><span class="sxs-lookup"><span data-stu-id="03e4a-164">Click **Add**.</span></span>

    <span data-ttu-id="03e4a-165">![Okno dialogowe Dodawanie kontrolera](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Okno dialogowe Dodawanie kontrolera")</span><span class="sxs-lookup"><span data-stu-id="03e4a-165">![Add controller dialog](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Add controller dialog")</span></span>

    <span data-ttu-id="03e4a-166">*Okno dialogowe Dodawanie kontrolera*</span><span class="sxs-lookup"><span data-stu-id="03e4a-166">*Add Controller Dialog*</span></span>

    <span data-ttu-id="03e4a-167">Jest generowana nowa klasa kontrolera.</span><span class="sxs-lookup"><span data-stu-id="03e4a-167">A new Controller class is generated.</span></span> <span data-ttu-id="03e4a-168">Ponieważ wskazane jest dodanie akcji do odczytu/zapisu, metody zastępcze dla tych, typowe akcje CRUD są tworzone z wypełnionymi komentarzami do wykonania, monitując o uwzględnienie logiki specyficznej dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="03e4a-168">Since you indicated to add actions for read/write, stub methods for those, common CRUD actions are created with TODO comments filled in, prompting to include the application specific logic.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a><span data-ttu-id="03e4a-169">Zadanie 2 — Dostosowywanie indeksu Magazynumanager</span><span class="sxs-lookup"><span data-stu-id="03e4a-169">Task 2 - Customizing the StoreManager Index</span></span>

<span data-ttu-id="03e4a-170">W tym zadaniu zostanie dostosowana Metoda działania indeksu Magazynumanager w celu zwrócenia widoku z listą albumów z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="03e4a-170">In this task, you will customize the StoreManager Index action method to return a View with the list of albums from the database.</span></span>

1. <span data-ttu-id="03e4a-171">W klasie StoreManagerController Dodaj następujące dyrektywy *using* .</span><span class="sxs-lookup"><span data-stu-id="03e4a-171">In the StoreManagerController class, add the following *using* directives.</span></span>

    <span data-ttu-id="03e4a-172">(Fragment kodu- *ASP.NET pomocników MVC 4 i formularze i sprawdzanie poprawności — Ex1 przy użyciu MvcMusicStore*)</span><span class="sxs-lookup"><span data-stu-id="03e4a-172">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 using MvcMusicStore*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. <span data-ttu-id="03e4a-173">Dodaj pole do **StoreManagerController** , aby pomieścić wystąpienie elementu **MusicStoreEntities.**</span><span class="sxs-lookup"><span data-stu-id="03e4a-173">Add a field to the **StoreManagerController** to hold an instance of **MusicStoreEntities.**</span></span>

    <span data-ttu-id="03e4a-174">(Fragment kodu- *ASP.NET pomocników MVC 4 i formularze i sprawdzanie poprawności — Ex1 MusicStoreEntities*)</span><span class="sxs-lookup"><span data-stu-id="03e4a-174">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 MusicStoreEntities*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. <span data-ttu-id="03e4a-175">Zaimplementuj akcję indeks StoreManagerController, aby zwrócić widok z listą albumów.</span><span class="sxs-lookup"><span data-stu-id="03e4a-175">Implement the StoreManagerController Index action to return a View with the list of albums.</span></span>

    <span data-ttu-id="03e4a-176">Logika akcji kontrolera będzie bardzo podobna do akcji indeksu StoreController zapisaną wcześniej.</span><span class="sxs-lookup"><span data-stu-id="03e4a-176">The Controller action logic will be very similar to the StoreController's Index action written earlier.</span></span> <span data-ttu-id="03e4a-177">Użyj LINQ do pobrania wszystkich albumów, w tym informacji o gatunku i wykonawcy do wyświetlenia.</span><span class="sxs-lookup"><span data-stu-id="03e4a-177">Use LINQ to retrieve all albums, including Genre and Artist information for display.</span></span>

    <span data-ttu-id="03e4a-178">(Fragment kodu- *ASP.NET pomocników MVC 4 i formularze i sprawdzanie poprawności — Ex1 StoreManagerController index*)</span><span class="sxs-lookup"><span data-stu-id="03e4a-178">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 StoreManagerController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a><span data-ttu-id="03e4a-179">Zadanie 3 — Tworzenie widoku indeksu</span><span class="sxs-lookup"><span data-stu-id="03e4a-179">Task 3 - Creating the Index View</span></span>

<span data-ttu-id="03e4a-180">W tym zadaniu utworzysz szablon widoku indeksu, aby wyświetlić listę albumów zwracanych przez kontroler **magazynu** .</span><span class="sxs-lookup"><span data-stu-id="03e4a-180">In this task, you will create the Index View template to display the list of albums returned by the **StoreManager** Controller.</span></span>

1. <span data-ttu-id="03e4a-181">Przed utworzeniem nowego szablonu widoku należy skompilować projekt, aby **okno dialogowe Dodawanie widoku** znało informacje o klasie **albumu** do użycia.</span><span class="sxs-lookup"><span data-stu-id="03e4a-181">Before creating the new View template, you should build the project so that the **Add View Dialog** knows about the **Album** class to use.</span></span> <span data-ttu-id="03e4a-182">Wybierz **kompilację | Kompiluj MvcMusicStore** w celu skompilowania projektu.</span><span class="sxs-lookup"><span data-stu-id="03e4a-182">Select **Build | Build MvcMusicStore** to build the project.</span></span>
2. <span data-ttu-id="03e4a-183">Kliknij prawym przyciskiem myszy wewnątrz metody akcji **indeksu** i wybierz polecenie **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-183">Right-click inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="03e4a-184">Spowoduje to wyświetlenie okna dialogowego **Dodawanie widoku** .</span><span class="sxs-lookup"><span data-stu-id="03e4a-184">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="03e4a-185">![Dodaj widok](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Dodaj widok")</span><span class="sxs-lookup"><span data-stu-id="03e4a-185">![Add view](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Add view")</span></span>

    <span data-ttu-id="03e4a-186">*Dodawanie widoku z poziomu metody index*</span><span class="sxs-lookup"><span data-stu-id="03e4a-186">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="03e4a-187">W oknie dialogowym Dodawanie widoku Sprawdź, czy nazwa widoku jest **indeksowana**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-187">In the Add View dialog, verify that the View Name is **Index**.</span></span> <span data-ttu-id="03e4a-188">Wybierz opcję **Utwórz widok z silną typem** i wybierz pozycję **album (MvcMusicStore. models)** z listy rozwijanej **Klasa modelu** .</span><span class="sxs-lookup"><span data-stu-id="03e4a-188">Select the **Create a strongly-typed view** option, and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="03e4a-189">Wybierz pozycję **Lista** na liście rozwijanej **szablon szkieletu** .</span><span class="sxs-lookup"><span data-stu-id="03e4a-189">Select **List** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="03e4a-190">Pozostaw **aparat widoku** do **Razor** i inne pola z wartościami domyślnymi, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-190">Leave the **View engine** to **Razor** and the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="03e4a-191">![Dodawanie widoku indeksu](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Dodawanie widoku indeksu")</span><span class="sxs-lookup"><span data-stu-id="03e4a-191">![Adding an index view](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Adding an index view")</span></span>

    <span data-ttu-id="03e4a-192">*Dodawanie widoku indeksu*</span><span class="sxs-lookup"><span data-stu-id="03e4a-192">*Adding an Index View*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a><span data-ttu-id="03e4a-193">Zadanie 4 — Dostosowywanie szkieletu widoku indeksu</span><span class="sxs-lookup"><span data-stu-id="03e4a-193">Task 4 - Customizing the scaffold of the Index View</span></span>

<span data-ttu-id="03e4a-194">W tym zadaniu dostosowano prosty szablon widoku utworzony przy użyciu funkcji szkieletowej ASP.NET MVC, aby wyświetlała odpowiednie pola.</span><span class="sxs-lookup"><span data-stu-id="03e4a-194">In this task, you will adjust the simple View template created with ASP.NET MVC scaffolding feature to have it display the fields you want.</span></span>

> [!NOTE]
> <span data-ttu-id="03e4a-195">Obsługa tworzenia **szkieletów** w ramach ASP.NET MVC generuje prosty szablon widoku, który zawiera listę wszystkich pól w modelu albumu.</span><span class="sxs-lookup"><span data-stu-id="03e4a-195">The **scaffolding** support within ASP.NET MVC generates a simple View template which lists all fields in the Album model.</span></span> <span data-ttu-id="03e4a-196">Tworzenie **szkieletów** umożliwia szybkie rozpoczęcie pracy w widoku o jednoznacznie określonym typie: zamiast konieczności ręcznego pisania szablonu widoku, tworzenie szkieletu szybko generuje szablon domyślny, a następnie można zmodyfikować wygenerowany kod.</span><span class="sxs-lookup"><span data-stu-id="03e4a-196">**Scaffolding** provides a quick way to get started on a strongly typed view: rather than having to write the View template manually, scaffolding quickly generates a default template and then you can modify the generated code.</span></span>

1. <span data-ttu-id="03e4a-197">Przejrzyj utworzony kod.</span><span class="sxs-lookup"><span data-stu-id="03e4a-197">Review the code created.</span></span> <span data-ttu-id="03e4a-198">Wygenerowana Lista pól będzie częścią poniższej tabeli **HTML, która** jest używana do wyświetlania danych tabelarycznych.</span><span class="sxs-lookup"><span data-stu-id="03e4a-198">The generated list of fields will be part of the following HTML table that **Scaffolding** is using for displaying tabular data.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. <span data-ttu-id="03e4a-199">Zastąp **&lt;tabelę&gt;** kodem następującym kodem, aby wyświetlić tylko pola **gatunek**, **wykonawca**, **tytuł albumu**i **Price** .</span><span class="sxs-lookup"><span data-stu-id="03e4a-199">Replace the **&lt;table&gt;** code with the following code to display only the **Genre**, **Artist**, **Album Title**, and **Price** fields.</span></span> <span data-ttu-id="03e4a-200">Spowoduje to usunięcie kolumn **AlbumId** i **URL albumów** .</span><span class="sxs-lookup"><span data-stu-id="03e4a-200">This deletes the **AlbumId** and **Album Art URL** columns.</span></span> <span data-ttu-id="03e4a-201">Ponadto zmienia kolumny GenreId i ArtistId, aby wyświetlić właściwości klasy połączonej **Artist.Name** i **Genre.Name**, a także usunąć łącze **szczegóły** .</span><span class="sxs-lookup"><span data-stu-id="03e4a-201">Also, it changes GenreId and ArtistId columns to display their linked class properties of **Artist.Name** and **Genre.Name**, and removes the **Details** link.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. <span data-ttu-id="03e4a-202">Zmień poniższe opisy.</span><span class="sxs-lookup"><span data-stu-id="03e4a-202">Change the following descriptions.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="03e4a-203">Zadanie 5 — Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="03e4a-203">Task 5 - Running the Application</span></span>

<span data-ttu-id="03e4a-204">W tym zadaniu zostanie przetestowana, że w szablonie widoku **indeksu** **magazynumanager** zostanie wyświetlona lista albumów zgodnie z projektem poprzednich kroków.</span><span class="sxs-lookup"><span data-stu-id="03e4a-204">In this task, you will test that the **StoreManager** **Index** View template displays a list of albums according to the design of the previous steps.</span></span>

1. <span data-ttu-id="03e4a-205">Naciśnij klawisz **F5** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="03e4a-205">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="03e4a-206">Projekt zostanie uruchomiony na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="03e4a-206">The project starts in the Home page.</span></span> <span data-ttu-id="03e4a-207">Zmień adres URL na **/StoreManager** , aby sprawdzić, czy zostanie wyświetlona lista albumów pokazująca ich **tytuł**, **wykonawcę** i **gatunek**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-207">Change the URL to **/StoreManager** to verify that a list of albums is displayed, showing their **Title**, **Artist** and **Genre**.</span></span>

    <span data-ttu-id="03e4a-208">![Przeglądanie listy albumów](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "Przeglądanie listy albumów")</span><span class="sxs-lookup"><span data-stu-id="03e4a-208">![Browsing the list of albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "Browsing the list of albums")</span></span>

    <span data-ttu-id="03e4a-209">*Przeglądanie listy albumów*</span><span class="sxs-lookup"><span data-stu-id="03e4a-209">*Browsing the list of albums*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a><span data-ttu-id="03e4a-210">Ćwiczenie 2: Dodawanie pomocnika HTML</span><span class="sxs-lookup"><span data-stu-id="03e4a-210">Exercise 2: Adding an HTML Helper</span></span>

<span data-ttu-id="03e4a-211">Na stronie indeksu Magazynumanager występuje jeden potencjalny problem: właściwości nazwy tytułu i wykonawcy mogą być wystarczająco długie, aby wyrzucać formatowanie tabeli.</span><span class="sxs-lookup"><span data-stu-id="03e4a-211">The StoreManager Index page has one potential issue: Title and Artist Name properties can both be long enough to throw off the table formatting.</span></span> <span data-ttu-id="03e4a-212">W tym ćwiczeniu dowiesz się, jak dodać niestandardowy pomocnik HTML do obcięcia tego tekstu.</span><span class="sxs-lookup"><span data-stu-id="03e4a-212">In this exercise you will learn how to add a custom HTML helper to truncate that text.</span></span>

<span data-ttu-id="03e4a-213">Na poniższej ilustracji widać, jak format jest modyfikowany z powodu długości tekstu, gdy jest używany mały rozmiar przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="03e4a-213">In the following figure, you can see how the format is modified because of the length of the text when you use a small browser size.</span></span>

<span data-ttu-id="03e4a-214">![Przeglądanie listy albumów z obciętym tekstem](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "Przeglądanie listy albumów z obciętym tekstem")</span><span class="sxs-lookup"><span data-stu-id="03e4a-214">![Browsing the list of Albums with not truncated text](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "Browsing the list of Albums with not truncated text")</span></span>

<span data-ttu-id="03e4a-215">*Przeglądanie listy albumów z obciętym tekstem*</span><span class="sxs-lookup"><span data-stu-id="03e4a-215">*Browsing the list of Albums with not truncated text*</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a><span data-ttu-id="03e4a-216">Zadanie 1 — rozszerzanie pomocnika HTML</span><span class="sxs-lookup"><span data-stu-id="03e4a-216">Task 1 - Extending the HTML Helper</span></span>

<span data-ttu-id="03e4a-217">W tym zadaniu zostanie dodana nowa metoda **obcinania** do obiektu **HTML** uwidocznionego w widokach ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="03e4a-217">In this task, you will add a new method **Truncate** to the **HTML** object exposed within ASP.NET MVC Views.</span></span> <span data-ttu-id="03e4a-218">W tym celu należy zaimplementować **metodę rozszerzenia** do wbudowanej klasy **System. Web. MVC. HtmlHelper** dostarczonej przez ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="03e4a-218">To do this, you will implement an **extension method** to the built-in **System.Web.Mvc.HtmlHelper** class provided by ASP.NET MVC.</span></span>

> [!NOTE]
> <span data-ttu-id="03e4a-219">Aby dowiedzieć się więcej na temat **metod rozszerzających**, zobacz ten artykuł w witrynie MSDN.</span><span class="sxs-lookup"><span data-stu-id="03e4a-219">To read more about **Extension Methods**, please visit this msdn article.</span></span> <span data-ttu-id="03e4a-220">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span><span class="sxs-lookup"><span data-stu-id="03e4a-220">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span></span>

1. <span data-ttu-id="03e4a-221">Otwórz rozwiązanie **BEGIN** znajdujące się w lokalizacji **Source/Ex2-AddingAnHTMLHelper/BEGIN/** folder.</span><span class="sxs-lookup"><span data-stu-id="03e4a-221">Open the **Begin** solution located at **Source/Ex2-AddingAnHTMLHelper/Begin/** folder.</span></span> <span data-ttu-id="03e4a-222">W przeciwnym razie można nadal korzystać z rozwiązania **końcowego** uzyskanego przez zakończenie poprzedniego ćwiczenia.</span><span class="sxs-lookup"><span data-stu-id="03e4a-222">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="03e4a-223">Jeśli otwarto **udostępnione rozwiązanie** , należy pobrać brakujące pakiety NuGet przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="03e4a-223">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="03e4a-224">W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-224">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="03e4a-225">W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.</span><span class="sxs-lookup"><span data-stu-id="03e4a-225">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="03e4a-226">Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-226">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="03e4a-227">Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="03e4a-227">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="03e4a-228">Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu.</span><span class="sxs-lookup"><span data-stu-id="03e4a-228">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="03e4a-229">To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="03e4a-229">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="03e4a-230">Otwórz widok indeksu elementu Storemanager.</span><span class="sxs-lookup"><span data-stu-id="03e4a-230">Open StoreManager's Index View.</span></span> <span data-ttu-id="03e4a-231">W tym celu w Eksplorator rozwiązań rozwiń folder **widoki** , a następnie pozycję **storemanager** i Otwórz plik **index. cshtml** .</span><span class="sxs-lookup"><span data-stu-id="03e4a-231">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
3. <span data-ttu-id="03e4a-232">Dodaj następujący kod poniżej dyrektywy <strong>@model</strong> , aby zdefiniować metodę pomocnika <strong>obcinania</strong> .</span><span class="sxs-lookup"><span data-stu-id="03e4a-232">Add the following code below the <strong>@model</strong> directive to define the <strong>Truncate</strong> helper method.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a><span data-ttu-id="03e4a-233">Zadanie 2 — obcinanie tekstu na stronie</span><span class="sxs-lookup"><span data-stu-id="03e4a-233">Task 2 - Truncating Text in the Page</span></span>

<span data-ttu-id="03e4a-234">W tym zadaniu zostanie użyta metoda **Truncate** do obcięcia tekstu w szablonie widoku.</span><span class="sxs-lookup"><span data-stu-id="03e4a-234">In this task, you will use the **Truncate** method to truncate the text in the View template.</span></span>

1. <span data-ttu-id="03e4a-235">Otwórz widok indeksu elementu Storemanager.</span><span class="sxs-lookup"><span data-stu-id="03e4a-235">Open StoreManager's Index View.</span></span> <span data-ttu-id="03e4a-236">W tym celu w Eksplorator rozwiązań rozwiń folder **widoki** , a następnie pozycję **storemanager** i Otwórz plik **index. cshtml** .</span><span class="sxs-lookup"><span data-stu-id="03e4a-236">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
2. <span data-ttu-id="03e4a-237">Zastąp wiersze pokazujące **nazwę wykonawcy** i **tytuł**albumu.</span><span class="sxs-lookup"><span data-stu-id="03e4a-237">Replace the lines that show the **Artist Name** and Album's **Title**.</span></span> <span data-ttu-id="03e4a-238">Aby to zrobić, Zastąp poniższe wiersze.</span><span class="sxs-lookup"><span data-stu-id="03e4a-238">To do this, replace the following lines.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="03e4a-239">Zadanie 3 — Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="03e4a-239">Task 3 - Running the Application</span></span>

<span data-ttu-id="03e4a-240">W tym zadaniu zostanie przetestowany, że szablon widoku **indeksu** **Magazynumanager** obcina tytuł albumu i nazwę wykonawcy.</span><span class="sxs-lookup"><span data-stu-id="03e4a-240">In this task, you will test that the **StoreManager** **Index** View template truncates the Album's Title and Artist Name.</span></span>

1. <span data-ttu-id="03e4a-241">Naciśnij klawisz **F5** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="03e4a-241">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="03e4a-242">Projekt zostanie uruchomiony na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="03e4a-242">The project starts in the Home page.</span></span> <span data-ttu-id="03e4a-243">Zmień adres URL na **/StoreManager** , aby sprawdzić, czy długie teksty w **tytule** i kolumnie **wykonawcy** zostały obcięte.</span><span class="sxs-lookup"><span data-stu-id="03e4a-243">Change the URL to **/StoreManager** to verify that long texts in the **Title** and **Artist** column are truncated.</span></span>

    <span data-ttu-id="03e4a-244">![Nazwy tytułów i artystów](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "Nazwy tytułów i artystów")</span><span class="sxs-lookup"><span data-stu-id="03e4a-244">![Truncated titles and artists names](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "Truncated titles and artists names")</span></span>

    <span data-ttu-id="03e4a-245">*Tytuły i nazwy wykonawców*</span><span class="sxs-lookup"><span data-stu-id="03e4a-245">*Truncated Titles and Artist Names*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a><span data-ttu-id="03e4a-246">Ćwiczenie 3: Tworzenie widoku edycji</span><span class="sxs-lookup"><span data-stu-id="03e4a-246">Exercise 3: Creating the Edit View</span></span>

<span data-ttu-id="03e4a-247">W tym ćwiczeniu dowiesz się, jak utworzyć formularz umożliwiający menedżerom magazynu edytowanie albumu.</span><span class="sxs-lookup"><span data-stu-id="03e4a-247">In this exercise, you will learn how to create a form to allow store managers to edit an Album.</span></span> <span data-ttu-id="03e4a-248">Przeszukamy adres URL **/StoreManager/Edit/ID** (**Identyfikator** jest unikatowym identyfikatorem albumu do edycji), co spowoduje wywołanie http-get do serwera.</span><span class="sxs-lookup"><span data-stu-id="03e4a-248">They will browse the **/StoreManager/Edit/id** URL (**id** being the unique id of the album to edit), thus making an HTTP-GET call to the server.</span></span>

<span data-ttu-id="03e4a-249">Metoda akcji Edytuj kontrolera pobierze odpowiedni album z bazy danych, utworzyć obiekt **StoreManagerViewModel** do hermetyzacji (wraz z listą artystów i gatunków), a następnie przekazać go do szablonu widoku w celu renderowania strony HTML z powrotem do użytkownika.</span><span class="sxs-lookup"><span data-stu-id="03e4a-249">The Controller Edit action method will retrieve the appropriate Album from the database, create a **StoreManagerViewModel** object to encapsulate it (along with a list of Artists and Genres), and then pass it off to a View template to render the HTML page back to the user.</span></span> <span data-ttu-id="03e4a-250">Ta strona będzie zawierać **&lt;formularz&gt;** z polami textlist i listami rozwijanymi służącymi do edytowania właściwości albumu.</span><span class="sxs-lookup"><span data-stu-id="03e4a-250">This page will contain a **&lt;form&gt;** element with textboxes and dropdowns for editing the Album properties.</span></span>

<span data-ttu-id="03e4a-251">Gdy użytkownik zaktualizuje wartości formularza albumu i kliknie przycisk **Zapisz** , zmiany są przesyłane za pośrednictwem wywołania http-post z powrotem do **/StoreManager/Edit/ID**. Mimo że adres URL pozostaje taki sam jak w przypadku ostatniego wywołania, ASP.NET MVC określa, że jest to czas POST protokołu HTTP i w związku z tym wykonuje inną metodę edycji akcji (z jedną dekoracyjną z **[HTTPPOST]** ).</span><span class="sxs-lookup"><span data-stu-id="03e4a-251">Once the user updates the Album form values and clicks the **Save** button, the changes are submitted via an HTTP-POST call back to **/StoreManager/Edit/id**. Although the URL remains the same as in the last call, ASP.NET MVC identifies that this time it is an HTTP-POST and therefore executes a different Edit action method (one decorated with **[HttpPost]**).</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a><span data-ttu-id="03e4a-252">Zadanie 1 — implementacja metody akcji edycji HTTP-GET</span><span class="sxs-lookup"><span data-stu-id="03e4a-252">Task 1 - Implementing the HTTP-GET Edit Action Method</span></span>

<span data-ttu-id="03e4a-253">W tym zadaniu zostanie zaimplementowana wersja protokołu HTTP-GET akcji Edytuj akcję w celu pobrania odpowiedniego albumu z bazy danych, a także listy wszystkich gatunków i artystów.</span><span class="sxs-lookup"><span data-stu-id="03e4a-253">In this task, you will implement the HTTP-GET version of the Edit action method to retrieve the appropriate Album from the database, as well as a list of all Genres and Artists.</span></span> <span data-ttu-id="03e4a-254">Spowoduje to spakowanie tych danych do obiektu **StoreManagerViewModel** zdefiniowanego w ostatnim kroku, który następnie zostanie przesłany do szablonu widoku w celu renderowania odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="03e4a-254">It will package this data up into the **StoreManagerViewModel** object defined in the last step, which will then be passed to a View template to render the response with.</span></span>

1. <span data-ttu-id="03e4a-255">Otwórz rozwiązanie **BEGIN** znajdujące się w lokalizacji **Source/Ex3-CreatingTheEditView/BEGIN/** folder.</span><span class="sxs-lookup"><span data-stu-id="03e4a-255">Open the **Begin** solution located at **Source/Ex3-CreatingTheEditView/Begin/** folder.</span></span> <span data-ttu-id="03e4a-256">W przeciwnym razie można nadal korzystać z rozwiązania **końcowego** uzyskanego przez zakończenie poprzedniego ćwiczenia.</span><span class="sxs-lookup"><span data-stu-id="03e4a-256">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="03e4a-257">Jeśli otwarto **udostępnione rozwiązanie** , należy pobrać brakujące pakiety NuGet przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="03e4a-257">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="03e4a-258">W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-258">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="03e4a-259">W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.</span><span class="sxs-lookup"><span data-stu-id="03e4a-259">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="03e4a-260">Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-260">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="03e4a-261">Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="03e4a-261">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="03e4a-262">Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu.</span><span class="sxs-lookup"><span data-stu-id="03e4a-262">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="03e4a-263">To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="03e4a-263">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="03e4a-264">Otwórz klasę **StoreManagerController** .</span><span class="sxs-lookup"><span data-stu-id="03e4a-264">Open the **StoreManagerController** class.</span></span> <span data-ttu-id="03e4a-265">Aby to zrobić, rozwiń folder **controllers** , a następnie kliknij dwukrotnie pozycję **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-265">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="03e4a-266">Zastąp metodę **http-Get Edit** Action następującym kodem, aby pobrać odpowiedni **album** , a także listę **gatunku** i **artystów** .</span><span class="sxs-lookup"><span data-stu-id="03e4a-266">Replace the **HTTP-GET Edit** action method with the following code to retrieve the appropriate **Album** as well as the **Genres** and **Artists** lists.</span></span>

    <span data-ttu-id="03e4a-267">(Fragment kodu- *ASP.NET pomocników MVC 4 i formularze i sprawdzanie poprawności — Ex3 STOREMANAGERCONTROLLER HTTP-GET Edycja akcji*)</span><span class="sxs-lookup"><span data-stu-id="03e4a-267">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-GET Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="03e4a-268">Używasz programu **System. Web. MVC** **SelectList** dla artystów i gatunek zamiast listy **System. Collections. Generic** .</span><span class="sxs-lookup"><span data-stu-id="03e4a-268">You are using **System.Web.Mvc** **SelectList** for Artists and Genres instead of the **System.Collections.Generic** List.</span></span>
    > 
    > <span data-ttu-id="03e4a-269">**SelectList** jest przejrzystym sposobem wypełniania list rozwijanych HTML i zarządzania elementami, takimi jak bieżące zaznaczenie.</span><span class="sxs-lookup"><span data-stu-id="03e4a-269">**SelectList** is a cleaner way to populate HTML dropdowns and manage things like current selection.</span></span> <span data-ttu-id="03e4a-270">Utworzenie wystąpienia i późniejsze skonfigurowanie tych obiektów ViewModel w akcji kontrolera spowoduje, że będzie to bardziej przejrzysty scenariusz edycji.</span><span class="sxs-lookup"><span data-stu-id="03e4a-270">Instantiating and later setting up these ViewModel objects in the controller action will make the Edit form scenario cleaner.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a><span data-ttu-id="03e4a-271">Zadanie 2 — Tworzenie widoku edycji</span><span class="sxs-lookup"><span data-stu-id="03e4a-271">Task 2 - Creating the Edit View</span></span>

<span data-ttu-id="03e4a-272">W tym zadaniu utworzysz szablon widoku edycji, który będzie później wyświetlał właściwości albumu.</span><span class="sxs-lookup"><span data-stu-id="03e4a-272">In this task, you will create an Edit View template that will later display the album properties.</span></span>

1. <span data-ttu-id="03e4a-273">Utwórz widok edycji.</span><span class="sxs-lookup"><span data-stu-id="03e4a-273">Create the Edit View.</span></span> <span data-ttu-id="03e4a-274">Aby to zrobić, kliknij prawym przyciskiem myszy wewnątrz metody akcji **Edytuj** i wybierz polecenie **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-274">To do this, right-click inside the **Edit** action method and select **Add View**.</span></span>
2. <span data-ttu-id="03e4a-275">W oknie dialogowym Dodawanie widoku Sprawdź, czy nazwa widoku jest **edytowana**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-275">In the Add View dialog, verify that the View Name is **Edit**.</span></span> <span data-ttu-id="03e4a-276">Zaznacz pole wyboru **Utwórz widok o jednoznacznie określonym typie** i wybierz opcję **album (MvcMusicStore. models)** z listy rozwijanej **Wyświetl klasę danych** .</span><span class="sxs-lookup"><span data-stu-id="03e4a-276">Check the **Create a strongly-typed view** checkbox and select **Album (MvcMusicStore.Models)** from the **View data class** drop-down.</span></span> <span data-ttu-id="03e4a-277">Wybierz pozycję **Edytuj** z listy rozwijanej **szablon szkieletu** .</span><span class="sxs-lookup"><span data-stu-id="03e4a-277">Select **Edit** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="03e4a-278">Pozostaw pozostałe pola wartościami domyślnymi, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-278">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="03e4a-279">![Dodawanie widoku edycji](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Dodawanie widoku edycji")</span><span class="sxs-lookup"><span data-stu-id="03e4a-279">![Adding an Edit view](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Adding an Edit view")</span></span>

    <span data-ttu-id="03e4a-280">*Dodawanie widoku edycji*</span><span class="sxs-lookup"><span data-stu-id="03e4a-280">*Adding an Edit view*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="03e4a-281">Zadanie 3 — Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="03e4a-281">Task 3 - Running the Application</span></span>

<span data-ttu-id="03e4a-282">W tym zadaniu przetestujesz, że na **stronie widok programu** **storemanager** są wyświetlane wartości właściwości dla albumu przesłanego jako parametr.</span><span class="sxs-lookup"><span data-stu-id="03e4a-282">In this task, you will test that the **StoreManager** **Edit** View page displays the properties' values for the album passed as parameter.</span></span>

1. <span data-ttu-id="03e4a-283">Naciśnij klawisz **F5** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="03e4a-283">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="03e4a-284">Projekt zostanie uruchomiony na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="03e4a-284">The project starts in the Home page.</span></span> <span data-ttu-id="03e4a-285">Zmień adres URL na **/StoreManager/Edit/1** , aby sprawdzić, czy są wyświetlane wartości właściwości dla przesłanego albumu.</span><span class="sxs-lookup"><span data-stu-id="03e4a-285">Change the URL to **/StoreManager/Edit/1** to verify that the properties' values for the album passed are displayed.</span></span>

    <span data-ttu-id="03e4a-286">![Przeglądanie widoku edycji albumu](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Przeglądanie widoku edycji albumu")</span><span class="sxs-lookup"><span data-stu-id="03e4a-286">![Browsing Album's Edit View](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Browsing Album's Edit View")</span></span>

    <span data-ttu-id="03e4a-287">*Przeglądanie widoku edycji albumu*</span><span class="sxs-lookup"><span data-stu-id="03e4a-287">*Browsing Album's Edit view*</span></span>

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a><span data-ttu-id="03e4a-288">Zadanie 4 — implementowanie list rozwijanych w szablonie edytora albumu</span><span class="sxs-lookup"><span data-stu-id="03e4a-288">Task 4 - Implementing drop-downs on the Album Editor Template</span></span>

<span data-ttu-id="03e4a-289">W tym zadaniu dodasz listę rozwijaną do szablonu widoku utworzonego w ostatnim zadaniu, dzięki czemu użytkownik może wybrać spośród listy artystów i gatunku.</span><span class="sxs-lookup"><span data-stu-id="03e4a-289">In this task, you will add drop-downs to the View template created in the last task, so that the user can select from a list of Artists and Genres.</span></span>

1. <span data-ttu-id="03e4a-290">Zastąp cały kod zestawu pól **albumu** następującym:</span><span class="sxs-lookup"><span data-stu-id="03e4a-290">Replace all the **Album** fieldset code with the following:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="03e4a-291">Dodano pomocnika **HTML. DropDownList** do listy rozwijanej renderowania do wybierania artystów i gatunków.</span><span class="sxs-lookup"><span data-stu-id="03e4a-291">An **Html.DropDownList** helper has been added to render drop-downs for choosing Artists and Genres.</span></span> <span data-ttu-id="03e4a-292">Parametry przesłane do **języka HTML. DropDownList** są następujące:</span><span class="sxs-lookup"><span data-stu-id="03e4a-292">The parameters passed to **Html.DropDownList** are:</span></span>
    > 
    > 1. <span data-ttu-id="03e4a-293">Nazwa pola formularza ( **&quot;ArtistId&quot;** ).</span><span class="sxs-lookup"><span data-stu-id="03e4a-293">The name of the form field (**&quot;ArtistId&quot;**).</span></span>
    > 2. <span data-ttu-id="03e4a-294">**SelectList** wartości dla listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="03e4a-294">The **SelectList** of values for the drop-down.</span></span>

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="03e4a-295">Zadanie 5 — Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="03e4a-295">Task 5 - Running the Application</span></span>

<span data-ttu-id="03e4a-296">W tym zadaniu zostanie przetestowany, że na **stronie widok widoku** **sklepumanager** są wyświetlane listy rozwijane zamiast pól tekstowych Nazwa wykonawca i gatunek.</span><span class="sxs-lookup"><span data-stu-id="03e4a-296">In this task, you will test that the **StoreManager** **Edit** View page displays drop-downs instead of Artist and Genre ID text fields.</span></span>

1. <span data-ttu-id="03e4a-297">Naciśnij klawisz **F5** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="03e4a-297">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="03e4a-298">Projekt zostanie uruchomiony na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="03e4a-298">The project starts in the Home page.</span></span> <span data-ttu-id="03e4a-299">Zmień adres URL na **/StoreManager/Edit/1** , aby upewnić się, że wyświetla listę rozwijaną zamiast pól tekstowych Nazwa wykonawca i gatunek.</span><span class="sxs-lookup"><span data-stu-id="03e4a-299">Change the URL to **/StoreManager/Edit/1** to verify that it displays drop-downs instead of Artist and Genre ID text fields.</span></span>

    <span data-ttu-id="03e4a-300">![Przeglądanie widoku edycji albumu z listami rozwijanymi](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Przeglądanie widoku edycji albumu z listami rozwijanymi")</span><span class="sxs-lookup"><span data-stu-id="03e4a-300">![Browsing Album's Edit View with drop downs](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Browsing Album's Edit View with drop downs")</span></span>

    <span data-ttu-id="03e4a-301">*Przeglądanie widoku edycji albumu, ten czas z listami rozwijanymi*</span><span class="sxs-lookup"><span data-stu-id="03e4a-301">*Browsing Album's Edit view, this time with dropdowns*</span></span>

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a><span data-ttu-id="03e4a-302">Zadanie 6 — implementacja metody akcji edycji HTTP-POST</span><span class="sxs-lookup"><span data-stu-id="03e4a-302">Task 6 - Implementing the HTTP-POST Edit action method</span></span>

<span data-ttu-id="03e4a-303">Teraz, gdy widok edycji jest wyświetlany zgodnie z oczekiwaniami, należy zaimplementować metodę akcji edycji HTTP-POST w celu zapisania zmian wprowadzonych w albumie.</span><span class="sxs-lookup"><span data-stu-id="03e4a-303">Now that the Edit View displays as expected, you need to implement the HTTP-POST Edit Action method to save the changes made to the Album.</span></span>

1. <span data-ttu-id="03e4a-304">W razie potrzeby Zamknij przeglądarkę, aby powrócić do okna programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="03e4a-304">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="03e4a-305">Otwórz **StoreManagerController** z folderu **controllers** .</span><span class="sxs-lookup"><span data-stu-id="03e4a-305">Open **StoreManagerController** from the **Controllers** folder.</span></span>
2. <span data-ttu-id="03e4a-306">Zamień kod metody akcji **edycji http-post** na następujący (należy zauważyć, że metoda, która musi zostać zastąpiona, ma przeciążoną wersję, która odbiera dwa parametry):</span><span class="sxs-lookup"><span data-stu-id="03e4a-306">Replace **HTTP-POST Edit** action method code with the following (note that the method that must be replaced is overloaded version that receives two parameters):</span></span>

    <span data-ttu-id="03e4a-307">(Fragment kodu- *ASP.NET pomocników MVC 4 i formularze i sprawdzanie poprawności — Ex3 STOREMANAGERCONTROLLER http-post Edycja*)</span><span class="sxs-lookup"><span data-stu-id="03e4a-307">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-POST Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="03e4a-308">Ta metoda zostanie wykonana po kliknięciu przez użytkownika przycisku **Zapisz** i przeniesieniu http-post wartości formularza z powrotem na serwer w celu utrwalenia ich w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="03e4a-308">This method will be executed when the user clicks the **Save** button of the View and performs an HTTP-POST of the form values back to the server to persist them in the database.</span></span> <span data-ttu-id="03e4a-309">Dekoratora **[HTTPPOST]** wskazuje, że metoda powinna być stosowana dla tych scenariuszy http-post.</span><span class="sxs-lookup"><span data-stu-id="03e4a-309">The decorator **[HttpPost]** indicates that the method should be used for those HTTP-POST scenarios.</span></span> <span data-ttu-id="03e4a-310">Metoda przyjmuje obiekt **albumu** .</span><span class="sxs-lookup"><span data-stu-id="03e4a-310">The method takes an **Album** object.</span></span> <span data-ttu-id="03e4a-311">ASP.NET MVC automatycznie utworzy obiekt albumu na podstawie &lt;formularza&gt; wartości.</span><span class="sxs-lookup"><span data-stu-id="03e4a-311">ASP.NET MVC will automatically create the Album object from the posted &lt;form&gt; values.</span></span>
    > 
    > <span data-ttu-id="03e4a-312">Ta metoda wykona następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="03e4a-312">The method will perform these steps:</span></span>
    > 
    > 1. <span data-ttu-id="03e4a-313">Jeśli model jest prawidłowy:</span><span class="sxs-lookup"><span data-stu-id="03e4a-313">If model is valid:</span></span>
    > 
    >     1. <span data-ttu-id="03e4a-314">Zaktualizuj wpis albumu w kontekście, aby oznaczyć go jako zmodyfikowany obiekt.</span><span class="sxs-lookup"><span data-stu-id="03e4a-314">Update the album entry in the context to mark it as a modified object.</span></span>
    >     2. <span data-ttu-id="03e4a-315">Zapisz zmiany i Przekieruj do widoku indeksu.</span><span class="sxs-lookup"><span data-stu-id="03e4a-315">Save the changes and redirect to the index view.</span></span>
    > 2. <span data-ttu-id="03e4a-316">Jeśli model jest nieprawidłowy, wypełni ViewBag z **GenreId** i **ArtistId**, a następnie zwróci widok z odebranym obiektem albumu, aby zezwolić użytkownikowi na wykonanie dowolnej wymaganej aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="03e4a-316">If the model is not valid, it will populate the ViewBag with the **GenreId** and **ArtistId**, then it will return the view with the received Album object to allow the user perform any required update.</span></span>

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="03e4a-317">Zadanie 7 — Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="03e4a-317">Task 7 - Running the Application</span></span>

<span data-ttu-id="03e4a-318">W tym zadaniu zostanie przetestowana, że na stronie widok **magazynumanager** , w rzeczywistości zapisywane są zaktualizowane dane albumu w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="03e4a-318">In this task, you will test that the **StoreManager Edit** View page actually saves the updated Album data in the database.</span></span>

1. <span data-ttu-id="03e4a-319">Naciśnij klawisz **F5** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="03e4a-319">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="03e4a-320">Projekt zostanie uruchomiony na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="03e4a-320">The project starts in the Home page.</span></span> <span data-ttu-id="03e4a-321">Zmień adres URL na **/StoreManager/Edit/1**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-321">Change the URL to **/StoreManager/Edit/1**.</span></span> <span data-ttu-id="03e4a-322">Zmień tytuł albumu na **Załaduj** i kliknij pozycję **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-322">Change the Album title to **Load** and click on **Save**.</span></span> <span data-ttu-id="03e4a-323">Sprawdź, czy tytuł albumu został rzeczywiście zmieniony na liście albumów.</span><span class="sxs-lookup"><span data-stu-id="03e4a-323">Verify that album's title actually changed in the list of albums.</span></span>

    <span data-ttu-id="03e4a-324">![Aktualizowanie albumu](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "Aktualizowanie albumu")</span><span class="sxs-lookup"><span data-stu-id="03e4a-324">![Updating an album](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "Updating an album")</span></span>

    <span data-ttu-id="03e4a-325">*Aktualizowanie albumu*</span><span class="sxs-lookup"><span data-stu-id="03e4a-325">*Updating an Album*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a><span data-ttu-id="03e4a-326">Ćwiczenie 4: Dodawanie widoku tworzenia</span><span class="sxs-lookup"><span data-stu-id="03e4a-326">Exercise 4: Adding a Create View</span></span>

<span data-ttu-id="03e4a-327">Teraz, gdy **StoreManagerController** obsługuje możliwość **edycji** , w tym ćwiczeniu dowiesz się, jak dodać szablon tworzenia widoku, aby umożliwić menedżerom sklepu Dodawanie nowych albumów do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="03e4a-327">Now that the **StoreManagerController** supports the **Edit** ability, in this exercise you will learn how to add a Create View template to let store managers add new Albums to the application.</span></span>

<span data-ttu-id="03e4a-328">Podobnie jak w przypadku korzystania z funkcji edycji, należy zaimplementować scenariusz tworzenia przy użyciu dwóch oddzielnych metod w klasie **StoreManagerController** :</span><span class="sxs-lookup"><span data-stu-id="03e4a-328">Like you did with the Edit functionality, you will implement the Create scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="03e4a-329">Jedna z metod akcji będzie wyświetlała pusty formularz, gdy menedżerowie sklepu najpierw odwiedzają adres URL **/StoreManager/Create** .</span><span class="sxs-lookup"><span data-stu-id="03e4a-329">One action method will display an empty form when store managers first visit the **/StoreManager/Create** URL.</span></span>
2. <span data-ttu-id="03e4a-330">Druga metoda działania będzie obsługiwać scenariusz, w którym Menedżer sklepu klika przycisk **Zapisz** w formularzu i przesyła wartości z powrotem do adresu URL **/StoreManager/Create** jako http-post.</span><span class="sxs-lookup"><span data-stu-id="03e4a-330">A second action method will handle the scenario where the store manager clicks the **Save** button within the form and submits the values back to the **/StoreManager/Create** URL as an HTTP-POST.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a><span data-ttu-id="03e4a-331">Zadanie 1 — implementacja metody działania HTTP-GET Create</span><span class="sxs-lookup"><span data-stu-id="03e4a-331">Task 1 - Implementing the HTTP-GET Create action method</span></span>

<span data-ttu-id="03e4a-332">W tym zadaniu zostanie zaimplementowana wersja protokołu HTTP-GET metody Create Action w celu pobrania listy wszystkich gatunków i artystów, spakowanie tych danych w obiekt **StoreManagerViewModel** , który zostanie następnie przesłany do szablonu widoku.</span><span class="sxs-lookup"><span data-stu-id="03e4a-332">In this task, you will implement the HTTP-GET version of the Create action method to retrieve a list of all Genres and Artists, package this data up into a **StoreManagerViewModel** object, which will then be passed to a View template.</span></span>

1. <span data-ttu-id="03e4a-333">Otwórz rozwiązanie **BEGIN** znajdujące się w lokalizacji **Source/EX4-AddingACreateView/BEGIN/** folder.</span><span class="sxs-lookup"><span data-stu-id="03e4a-333">Open the **Begin** solution located at **Source/Ex4-AddingACreateView/Begin/** folder.</span></span> <span data-ttu-id="03e4a-334">W przeciwnym razie można nadal korzystać z rozwiązania **końcowego** uzyskanego przez zakończenie poprzedniego ćwiczenia.</span><span class="sxs-lookup"><span data-stu-id="03e4a-334">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="03e4a-335">Jeśli otwarto **udostępnione rozwiązanie** , należy pobrać brakujące pakiety NuGet przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="03e4a-335">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="03e4a-336">W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-336">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="03e4a-337">W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.</span><span class="sxs-lookup"><span data-stu-id="03e4a-337">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="03e4a-338">Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-338">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="03e4a-339">Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="03e4a-339">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="03e4a-340">Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu.</span><span class="sxs-lookup"><span data-stu-id="03e4a-340">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="03e4a-341">To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="03e4a-341">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="03e4a-342">Otwórz klasę **StoreManagerController** .</span><span class="sxs-lookup"><span data-stu-id="03e4a-342">Open **StoreManagerController** class.</span></span> <span data-ttu-id="03e4a-343">Aby to zrobić, rozwiń folder **controllers** , a następnie kliknij dwukrotnie pozycję **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-343">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="03e4a-344">Zamień kod metody **tworzenia** akcji na następujący:</span><span class="sxs-lookup"><span data-stu-id="03e4a-344">Replace the **Create** action method code with the following:</span></span>

    <span data-ttu-id="03e4a-345">(Fragment kodu- *ASP.NET pomocników MVC 4 i formularze i sprawdzanie poprawności — EX4 STOREMANAGERCONTROLLER HTTP-GET Create Action*)</span><span class="sxs-lookup"><span data-stu-id="03e4a-345">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP-GET Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a><span data-ttu-id="03e4a-346">Zadanie 2 — Dodawanie widoku</span><span class="sxs-lookup"><span data-stu-id="03e4a-346">Task 2 - Adding the Create View</span></span>

<span data-ttu-id="03e4a-347">W tym zadaniu dodasz szablon Utwórz widok, który będzie wyświetlał nowy (pusty) formularz albumu.</span><span class="sxs-lookup"><span data-stu-id="03e4a-347">In this task, you will add the Create View template that will display a new (empty) Album form.</span></span>

1. <span data-ttu-id="03e4a-348">Kliknij prawym przyciskiem myszy wewnątrz metody **tworzenia** akcji i wybierz polecenie **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-348">Right-click inside the **Create** action method and select **Add View**.</span></span> <span data-ttu-id="03e4a-349">Spowoduje to wyświetlenie okna dialogowego Dodawanie widoku.</span><span class="sxs-lookup"><span data-stu-id="03e4a-349">This will bring up the Add View dialog.</span></span>
2. <span data-ttu-id="03e4a-350">W oknie dialogowym Dodawanie widoku Sprawdź, czy nazwa widoku jest **utworzona**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-350">In the Add View dialog, verify that the View Name is **Create**.</span></span> <span data-ttu-id="03e4a-351">Wybierz opcję **Utwórz widok z silną typem** i wybierz pozycję **album (MvcMusicStore. models)** z listy rozwijanej **Klasa modelu** i **Utwórz** z listy rozwijanej **szablon szkieletu** .</span><span class="sxs-lookup"><span data-stu-id="03e4a-351">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down and **Create** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="03e4a-352">Pozostaw pozostałe pola wartościami domyślnymi, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-352">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="03e4a-353">![Dodawanie widoku](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "Adding-a-CREATE-VIEW. png")</span><span class="sxs-lookup"><span data-stu-id="03e4a-353">![Adding a create view](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "adding-a-create-view.png")</span></span>

    <span data-ttu-id="03e4a-354">*Dodawanie widoku*</span><span class="sxs-lookup"><span data-stu-id="03e4a-354">*Adding the Create View*</span></span>
3. <span data-ttu-id="03e4a-355">Zaktualizuj pola **GenreId** i **ArtistId** , aby użyć listy rozwijanej, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="03e4a-355">Update the **GenreId** and **ArtistId** fields to use a drop-down list as shown below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="03e4a-356">Zadanie 3 — Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="03e4a-356">Task 3 - Running the Application</span></span>

<span data-ttu-id="03e4a-357">W tym zadaniu sprawdzisz, że na stronie **Tworzenie** widoku **magazynumanager** zostanie wyświetlony pusty formularz albumu.</span><span class="sxs-lookup"><span data-stu-id="03e4a-357">In this task, you will test that the **StoreManager** **Create** View page displays an empty Album form.</span></span>

1. <span data-ttu-id="03e4a-358">Naciśnij klawisz **F5** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="03e4a-358">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="03e4a-359">Projekt zostanie uruchomiony na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="03e4a-359">The project starts in the Home page.</span></span> <span data-ttu-id="03e4a-360">Zmień adres URL na **/StoreManager/Create**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-360">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="03e4a-361">Sprawdź, czy jest wyświetlany pusty formularz służący do wypełniania właściwości nowego albumu.</span><span class="sxs-lookup"><span data-stu-id="03e4a-361">Verify that an empty form is displayed for filling the new Album properties.</span></span>

    <span data-ttu-id="03e4a-362">![Utwórz widok z pustym formularzem](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Utwórz widok z pustym formularzem")</span><span class="sxs-lookup"><span data-stu-id="03e4a-362">![Create View with an empty form](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Create View with an empty form")</span></span>

    <span data-ttu-id="03e4a-363">*Utwórz widok z pustym formularzem*</span><span class="sxs-lookup"><span data-stu-id="03e4a-363">*Create View with an empty form*</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a><span data-ttu-id="03e4a-364">Zadanie 4 — implementacja metody działania HTTP-POST Create</span><span class="sxs-lookup"><span data-stu-id="03e4a-364">Task 4 - Implementing the HTTP-POST Create Action Method</span></span>

<span data-ttu-id="03e4a-365">W tym zadaniu zostanie zaimplementowana wersja HTTP-POST metody Create Action, która będzie wywoływana, gdy użytkownik kliknie przycisk **Zapisz** .</span><span class="sxs-lookup"><span data-stu-id="03e4a-365">In this task, you will implement the HTTP-POST version of the Create action method that will be invoked when a user clicks the **Save** button.</span></span> <span data-ttu-id="03e4a-366">Metoda powinna zapisać nowy album w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="03e4a-366">The method should save the new album in the database.</span></span>

1. <span data-ttu-id="03e4a-367">W razie potrzeby Zamknij przeglądarkę, aby powrócić do okna programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="03e4a-367">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="03e4a-368">Otwórz klasę **StoreManagerController** .</span><span class="sxs-lookup"><span data-stu-id="03e4a-368">Open **StoreManagerController** class.</span></span> <span data-ttu-id="03e4a-369">Aby to zrobić, rozwiń folder **controllers** , a następnie kliknij dwukrotnie pozycję **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-369">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="03e4a-370">Zamień kod metody akcji **Create http-post** na następujący:</span><span class="sxs-lookup"><span data-stu-id="03e4a-370">Replace **HTTP-POST Create** action method code with the following:</span></span>

    <span data-ttu-id="03e4a-371">(Fragment kodu- *ASP.NET pomocników MVC 4 i formularze i sprawdzanie poprawności — EX4 STOREMANAGERCONTROLLER http-post create Action*)</span><span class="sxs-lookup"><span data-stu-id="03e4a-371">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP- POST Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="03e4a-372">Akcja tworzenia jest całkiem podobna do poprzedniej metody edycji akcji, ale zamiast ustawiania obiektu jako zmodyfikowany, jest dodawany do kontekstu.</span><span class="sxs-lookup"><span data-stu-id="03e4a-372">The Create action is pretty similar to the previous Edit action method but instead of setting the object as modified, it is being added to the context.</span></span>

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="03e4a-373">Zadanie 5 — Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="03e4a-373">Task 5 - Running the Application</span></span>

<span data-ttu-id="03e4a-374">W tym zadaniu zostanie przetestowana, że strona **Tworzenie widoku magazynumanager** umożliwia utworzenie nowego albumu, a następnie przekierowanie do widoku indeksu magazynumanager.</span><span class="sxs-lookup"><span data-stu-id="03e4a-374">In this task, you will test that the **StoreManager Create** View page lets you create a new Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="03e4a-375">Naciśnij klawisz **F5** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="03e4a-375">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="03e4a-376">Projekt zostanie uruchomiony na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="03e4a-376">The project starts in the Home page.</span></span> <span data-ttu-id="03e4a-377">Zmień adres URL na **/StoreManager/Create**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-377">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="03e4a-378">Wypełnij wszystkie pola formularza danymi dla nowego albumu, tak jak na poniższej ilustracji:</span><span class="sxs-lookup"><span data-stu-id="03e4a-378">Fill all the form fields with data for a new Album, like the one in the following figure:</span></span>

    <span data-ttu-id="03e4a-379">![Tworzenie albumu](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "Tworzenie albumu")</span><span class="sxs-lookup"><span data-stu-id="03e4a-379">![Creating an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "Creating an Album")</span></span>

    <span data-ttu-id="03e4a-380">*Tworzenie albumu*</span><span class="sxs-lookup"><span data-stu-id="03e4a-380">*Creating an Album*</span></span>
3. <span data-ttu-id="03e4a-381">Upewnij się, że nastąpi przekierowanie do widoku indeksu Magazynumanager zawierającego nowy album właśnie utworzony.</span><span class="sxs-lookup"><span data-stu-id="03e4a-381">Verify that you get redirected to the StoreManager Index View that includes the new Album just created.</span></span>

    <span data-ttu-id="03e4a-382">![Utworzono nowy album](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "Utworzono nowy album")</span><span class="sxs-lookup"><span data-stu-id="03e4a-382">![New Album Created](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "New Album Created")</span></span>

    <span data-ttu-id="03e4a-383">*Utworzono nowy album*</span><span class="sxs-lookup"><span data-stu-id="03e4a-383">*New Album Created*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a><span data-ttu-id="03e4a-384">Ćwiczenie 5: obsługa usuwania</span><span class="sxs-lookup"><span data-stu-id="03e4a-384">Exercise 5: Handling Deletion</span></span>

<span data-ttu-id="03e4a-385">Możliwość usuwania albumów nie została jeszcze zaimplementowana.</span><span class="sxs-lookup"><span data-stu-id="03e4a-385">The ability to delete albums is not yet implemented.</span></span> <span data-ttu-id="03e4a-386">W tym ćwiczeniu będzie to możliwe.</span><span class="sxs-lookup"><span data-stu-id="03e4a-386">This is what this exercise will be about.</span></span> <span data-ttu-id="03e4a-387">Podobnie jak przed, zaimplementowano scenariusz usuwania przy użyciu dwóch oddzielnych metod w klasie **StoreManagerController** :</span><span class="sxs-lookup"><span data-stu-id="03e4a-387">Like before, you will implement the Delete scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="03e4a-388">Jedna metoda akcji będzie wyświetlać formularz potwierdzenia</span><span class="sxs-lookup"><span data-stu-id="03e4a-388">One action method will display a confirmation form</span></span>
2. <span data-ttu-id="03e4a-389">Druga metoda działania będzie obsługiwać przesyłanie formularza</span><span class="sxs-lookup"><span data-stu-id="03e4a-389">A second action method will handle the form submission</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a><span data-ttu-id="03e4a-390">Zadanie 1 — implementacja metody akcji usuwania HTTP-GET</span><span class="sxs-lookup"><span data-stu-id="03e4a-390">Task 1 - Implementing the HTTP-GET Delete Action Method</span></span>

<span data-ttu-id="03e4a-391">W tym zadaniu zostanie zaimplementowana wersja protokołu HTTP-GET metody usuwania akcji w celu pobrania informacji o albumie.</span><span class="sxs-lookup"><span data-stu-id="03e4a-391">In this task, you will implement the HTTP-GET version of the Delete action method to retrieve the album's information.</span></span>

1. <span data-ttu-id="03e4a-392">Otwórz rozwiązanie **BEGIN** znajdujące się w lokalizacji **Source/Ex5-HandlingDeletion/BEGIN/** folder.</span><span class="sxs-lookup"><span data-stu-id="03e4a-392">Open the **Begin** solution located at **Source/Ex5-HandlingDeletion/Begin/** folder.</span></span> <span data-ttu-id="03e4a-393">W przeciwnym razie można nadal korzystać z rozwiązania **końcowego** uzyskanego przez zakończenie poprzedniego ćwiczenia.</span><span class="sxs-lookup"><span data-stu-id="03e4a-393">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="03e4a-394">Jeśli otwarto **udostępnione rozwiązanie** , należy pobrać brakujące pakiety NuGet przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="03e4a-394">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="03e4a-395">W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-395">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="03e4a-396">W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.</span><span class="sxs-lookup"><span data-stu-id="03e4a-396">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="03e4a-397">Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-397">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="03e4a-398">Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="03e4a-398">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="03e4a-399">Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu.</span><span class="sxs-lookup"><span data-stu-id="03e4a-399">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="03e4a-400">To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="03e4a-400">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="03e4a-401">Otwórz klasę **StoreManagerController** .</span><span class="sxs-lookup"><span data-stu-id="03e4a-401">Open **StoreManagerController** class.</span></span> <span data-ttu-id="03e4a-402">Aby to zrobić, rozwiń folder **controllers** , a następnie kliknij dwukrotnie pozycję **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-402">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="03e4a-403">Akcja Usuń kontroler jest dokładnie taka sama jak akcja kontrolera poprzedniego sklepu szczegóły magazynu: wysyła zapytanie do obiektu **albumu** z bazy danych przy użyciu **identyfikatora** POdanego w adresie URL i zwraca odpowiedni **Widok**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-403">The Delete controller action is exactly the same as the previous Store Details controller action: it queries the **album** object from the database using the **id** provided in the URL and returns the appropriate **View**.</span></span> <span data-ttu-id="03e4a-404">Aby to zrobić, Zastąp kod metody akcji HTTP-GET **delete** następującym:</span><span class="sxs-lookup"><span data-stu-id="03e4a-404">To do this, replace the HTTP-GET **Delete** action method code with the following:</span></span>

    <span data-ttu-id="03e4a-405">(Fragment kodu- *ASP.NET pomocników MVC 4 i formularzy i walidacji — usuwanie Ex5 obsługa operacji HTTP-GET Delete*)</span><span class="sxs-lookup"><span data-stu-id="03e4a-405">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-GET Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. <span data-ttu-id="03e4a-406">Kliknij prawym przyciskiem myszy wewnątrz metody **usuwania** akcji i wybierz polecenie **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-406">Right-click inside the **Delete** action method and select **Add View**.</span></span> <span data-ttu-id="03e4a-407">Spowoduje to wyświetlenie okna dialogowego Dodawanie widoku.</span><span class="sxs-lookup"><span data-stu-id="03e4a-407">This will bring up the Add View dialog.</span></span>
5. <span data-ttu-id="03e4a-408">W oknie dialogowym Dodawanie widoku Sprawdź, czy nazwa widoku jest **usuwana**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-408">In the Add View dialog, verify that the View name is **Delete**.</span></span> <span data-ttu-id="03e4a-409">Wybierz opcję **Utwórz widok z silną typem** i wybierz pozycję **album (MvcMusicStore. models)** z listy rozwijanej **Klasa modelu** .</span><span class="sxs-lookup"><span data-stu-id="03e4a-409">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="03e4a-410">Wybierz pozycję **Usuń** z listy rozwijanej **szablon szkieletu** .</span><span class="sxs-lookup"><span data-stu-id="03e4a-410">Select **Delete** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="03e4a-411">Pozostaw pozostałe pola wartościami domyślnymi, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-411">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="03e4a-412">![Dodawanie widoku usuwania](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Dodawanie widoku usuwania")</span><span class="sxs-lookup"><span data-stu-id="03e4a-412">![Adding a Delete View](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Adding a Delete View")</span></span>

    <span data-ttu-id="03e4a-413">*Dodawanie widoku usuwania*</span><span class="sxs-lookup"><span data-stu-id="03e4a-413">*Adding a Delete View*</span></span>
6. <span data-ttu-id="03e4a-414">Szablon Usuń zawiera wszystkie pola z modelu.</span><span class="sxs-lookup"><span data-stu-id="03e4a-414">The Delete template shows all the fields from the model.</span></span> <span data-ttu-id="03e4a-415">Zostanie wyświetlony tylko tytuł albumu.</span><span class="sxs-lookup"><span data-stu-id="03e4a-415">You will show only the album's title.</span></span> <span data-ttu-id="03e4a-416">Aby to zrobić, Zastąp zawartość widoku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="03e4a-416">To do this, replace the content of the view with the following code:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="03e4a-417">Zadanie 2 — Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="03e4a-417">Task 2 - Running the Application</span></span>

<span data-ttu-id="03e4a-418">W tym zadaniu przetestujesz, że na stronie **usuwanie** widoku **magazynumanager** zostanie wyświetlony formularz usuwania potwierdzenia.</span><span class="sxs-lookup"><span data-stu-id="03e4a-418">In this task, you will test that the **StoreManager** **Delete** View page displays a confirmation deletion form.</span></span>

1. <span data-ttu-id="03e4a-419">Naciśnij klawisz **F5** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="03e4a-419">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="03e4a-420">Projekt zostanie uruchomiony na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="03e4a-420">The project starts in the Home page.</span></span> <span data-ttu-id="03e4a-421">Zmień adres URL na **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-421">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="03e4a-422">Wybierz jeden z albumów do usunięcia, klikając przycisk **Usuń** i sprawdź, czy nowy widok został przekazany.</span><span class="sxs-lookup"><span data-stu-id="03e4a-422">Select one album to delete by clicking **Delete** and verify that the new view is uploaded.</span></span>

    <span data-ttu-id="03e4a-423">![Usuwanie albumu](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "Usuwanie albumu")</span><span class="sxs-lookup"><span data-stu-id="03e4a-423">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "Deleting an Album")</span></span>

    <span data-ttu-id="03e4a-424">*Usuwanie albumu*</span><span class="sxs-lookup"><span data-stu-id="03e4a-424">*Deleting an Album*</span></span>

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a><span data-ttu-id="03e4a-425">Zadanie 3 — Implementacja metody akcji HTTP-POST Delete</span><span class="sxs-lookup"><span data-stu-id="03e4a-425">Task 3- Implementing the HTTP-POST Delete Action Method</span></span>

<span data-ttu-id="03e4a-426">W tym zadaniu zostanie zaimplementowana wersja HTTP-POST metody usuwania akcji, która będzie wywoływana, gdy użytkownik kliknie przycisk **Usuń** .</span><span class="sxs-lookup"><span data-stu-id="03e4a-426">In this task, you will implement the HTTP-POST version of the Delete action method that will be invoked when a user clicks the **Delete** button.</span></span> <span data-ttu-id="03e4a-427">Metoda powinna usunąć album w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="03e4a-427">The method should delete the album in the database.</span></span>

1. <span data-ttu-id="03e4a-428">W razie potrzeby Zamknij przeglądarkę, aby powrócić do okna programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="03e4a-428">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="03e4a-429">Otwórz klasę **StoreManagerController** .</span><span class="sxs-lookup"><span data-stu-id="03e4a-429">Open **StoreManagerController** class.</span></span> <span data-ttu-id="03e4a-430">Aby to zrobić, rozwiń folder **controllers** , a następnie kliknij dwukrotnie pozycję **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-430">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="03e4a-431">Zastąp kod metody akcji **usuwania http-post** następującym:</span><span class="sxs-lookup"><span data-stu-id="03e4a-431">Replace **HTTP-POST Delete** action method code with the following:</span></span>

    <span data-ttu-id="03e4a-432">(Fragment kodu- *ASP.NET pomocników MVC 4 i formularzy i weryfikacji — Ex5 obsługa usuwania operacji http-post*)</span><span class="sxs-lookup"><span data-stu-id="03e4a-432">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-POST Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="03e4a-433">Zadanie 4 — Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="03e4a-433">Task 4 - Running the Application</span></span>

<span data-ttu-id="03e4a-434">W tym zadaniu zostanie przetestowana Strona widok **usuwania widoku magazynumanager** umożliwia usunięcie albumu, a następnie przekierowanie do widoku indeksu magazynumanager.</span><span class="sxs-lookup"><span data-stu-id="03e4a-434">In this task, you will test that the **StoreManager Delete** View page lets you delete an Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="03e4a-435">Naciśnij klawisz **F5** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="03e4a-435">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="03e4a-436">Projekt zostanie uruchomiony na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="03e4a-436">The project starts in the Home page.</span></span> <span data-ttu-id="03e4a-437">Zmień adres URL na **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-437">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="03e4a-438">Wybierz jeden album do usunięcia, klikając przycisk **Usuń.**</span><span class="sxs-lookup"><span data-stu-id="03e4a-438">Select one album to delete by clicking **Delete.**</span></span> <span data-ttu-id="03e4a-439">Potwierdź usunięcie, klikając przycisk **Usuń** .</span><span class="sxs-lookup"><span data-stu-id="03e4a-439">Confirm the deletion by clicking **Delete** button:</span></span>

    <span data-ttu-id="03e4a-440">![Usuwanie albumu](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "Usuwanie albumu")</span><span class="sxs-lookup"><span data-stu-id="03e4a-440">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "Deleting an Album")</span></span>

    <span data-ttu-id="03e4a-441">*Usuwanie albumu*</span><span class="sxs-lookup"><span data-stu-id="03e4a-441">*Deleting an Album*</span></span>
3. <span data-ttu-id="03e4a-442">Sprawdź, czy album został usunięty, ponieważ nie jest wyświetlany na stronie **indeksu** .</span><span class="sxs-lookup"><span data-stu-id="03e4a-442">Verify that the album was deleted since it does not appear in the **Index** page.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a><span data-ttu-id="03e4a-443">Ćwiczenie 6: Dodawanie walidacji</span><span class="sxs-lookup"><span data-stu-id="03e4a-443">Exercise 6: Adding Validation</span></span>

<span data-ttu-id="03e4a-444">Obecnie w miejscu tworzenia i edytowania formularzy nie ma żadnego rodzaju walidacji.</span><span class="sxs-lookup"><span data-stu-id="03e4a-444">Currently, the Create and Edit forms you have in place do not perform any kind of validation.</span></span> <span data-ttu-id="03e4a-445">Jeśli użytkownik pozostawi pole wymagane puste lub wpisz litery w polu Cena, pierwszy z nich zostanie pobrany z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="03e4a-445">If the user leaves a required field blank or type letters in the price field, the first error you will get will be from the database.</span></span>

<span data-ttu-id="03e4a-446">Możesz dodać weryfikację do aplikacji, dodając adnotacje danych do klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="03e4a-446">You can add validation to the application by adding Data Annotations to your model class.</span></span> <span data-ttu-id="03e4a-447">Adnotacje danych umożliwiają opisywanie reguł, które mają być stosowane do właściwości modelu, a ASP.NET MVC zajmie się wymuszeniem i wyświetleniem odpowiedniego komunikatu dla użytkowników.</span><span class="sxs-lookup"><span data-stu-id="03e4a-447">Data Annotations allow describing the rules you want applied to your model properties, and ASP.NET MVC will take care of enforcing and displaying appropriate message to users.</span></span>

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a><span data-ttu-id="03e4a-448">Zadanie 1 — Dodawanie adnotacji do danych</span><span class="sxs-lookup"><span data-stu-id="03e4a-448">Task 1 - Adding Data Annotations</span></span>

<span data-ttu-id="03e4a-449">W tym zadaniu zostaną dodane adnotacje danych do modelu albumu, który spowoduje, że w razie potrzeby będzie wyświetlała się Strona tworzenie i edytowanie.</span><span class="sxs-lookup"><span data-stu-id="03e4a-449">In this task, you will add Data Annotations to the Album Model that will make the Create and Edit page display validation messages when appropriate.</span></span>

<span data-ttu-id="03e4a-450">W przypadku prostej klasy modelu Dodawanie adnotacji do danych jest obsługiwane tylko przez dodanie instrukcji **using** dla elementu **System. ComponentModel. DataAnnotation**, a następnie umieszczenie atrybutu **[Required]** w odpowiednich właściwościach.</span><span class="sxs-lookup"><span data-stu-id="03e4a-450">For a simple Model class, adding a Data Annotation is just handled by adding a **using** statement for **System.ComponentModel.DataAnnotation**, then placing a **[Required]** attribute on the appropriate properties.</span></span> <span data-ttu-id="03e4a-451">W poniższym przykładzie właściwość **name** jest wymagana w widoku.</span><span class="sxs-lookup"><span data-stu-id="03e4a-451">The following example would make the **Name** property a required field in the View.</span></span>

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

<span data-ttu-id="03e4a-452">Jest to nieco bardziej skomplikowany sposób, jak w przypadku tej aplikacji, w której jest generowana Entity Data Model.</span><span class="sxs-lookup"><span data-stu-id="03e4a-452">This is a little more complex in cases like this application where the Entity Data Model is generated.</span></span> <span data-ttu-id="03e4a-453">Jeśli dodano adnotacje do danych bezpośrednio do klas modelu, zostaną one nadpisywane, jeśli zaktualizujesz model z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="03e4a-453">If you added Data Annotations directly to the model classes, they would be overwritten if you update the model from the database.</span></span> <span data-ttu-id="03e4a-454">Zamiast tego można użyć klas częściowych metadanych, które będą istniały do przechowywania adnotacji i są skojarzone z klasami modelu przy użyciu atrybutu **[metadatatype]** .</span><span class="sxs-lookup"><span data-stu-id="03e4a-454">Instead, you can make use of metadata partial classes which will exist to hold the annotations and are associated with the model classes using the **[MetadataType]** attribute.</span></span>

1. <span data-ttu-id="03e4a-455">Otwórz rozwiązanie **BEGIN** znajdujące się w lokalizacji **Source/Ex6-AddingValidation/BEGIN/** folder.</span><span class="sxs-lookup"><span data-stu-id="03e4a-455">Open the **Begin** solution located at **Source/Ex6-AddingValidation/Begin/** folder.</span></span> <span data-ttu-id="03e4a-456">W przeciwnym razie można nadal korzystać z rozwiązania **końcowego** uzyskanego przez zakończenie poprzedniego ćwiczenia.</span><span class="sxs-lookup"><span data-stu-id="03e4a-456">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="03e4a-457">Jeśli otwarto **udostępnione rozwiązanie** , należy pobrać brakujące pakiety NuGet przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="03e4a-457">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="03e4a-458">W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-458">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="03e4a-459">W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.</span><span class="sxs-lookup"><span data-stu-id="03e4a-459">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="03e4a-460">Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-460">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="03e4a-461">Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="03e4a-461">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="03e4a-462">Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu.</span><span class="sxs-lookup"><span data-stu-id="03e4a-462">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="03e4a-463">To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="03e4a-463">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="03e4a-464">Otwórz **album.cs** z folderu **modele** .</span><span class="sxs-lookup"><span data-stu-id="03e4a-464">Open the **Album.cs** from the **Models** folder.</span></span>
3. <span data-ttu-id="03e4a-465">Zamień zawartość **album.cs** na wyróżniony kod, tak aby wyglądał wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="03e4a-465">Replace **Album.cs** content with the highlighted code, so that it looks like the following:</span></span>

    > [!NOTE]
    > <span data-ttu-id="03e4a-466">Wiersz **[DisplayFormat (ConvertEmptyStringToNull = false)]** wskazuje, że puste ciągi z modelu nie będą konwertowane na wartość null, gdy pole danych zostanie zaktualizowane w źródle danych.</span><span class="sxs-lookup"><span data-stu-id="03e4a-466">The line **[DisplayFormat(ConvertEmptyStringToNull=false)]** indicates that empty strings from the model won't be converted to null when the data field is updated in the data source.</span></span> <span data-ttu-id="03e4a-467">To ustawienie pozwala uniknąć wyjątku, gdy Entity Framework przypisuje wartości null do modelu, zanim adnotacja danych potwierdzi pola.</span><span class="sxs-lookup"><span data-stu-id="03e4a-467">This setting will avoid an exception when the Entity Framework assigns null values to the model before Data Annotation validates the fields.</span></span>

    <span data-ttu-id="03e4a-468">(Fragment kodu- *ASP.NET pomocników MVC 4 i formularzy i weryfikacji — Ex6 — Klasa częściowa metadanych albumu*)</span><span class="sxs-lookup"><span data-stu-id="03e4a-468">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex6 Album metadata partial class*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > <span data-ttu-id="03e4a-469">Ta klasa częściowego **albumu** ma atrybut **datadatatype** , który wskazuje klasę **AlbumMetaData** dla adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="03e4a-469">This **Album** partial class has a **MetadataType** attribute which points to the **AlbumMetaData** class for the Data Annotations.</span></span> <span data-ttu-id="03e4a-470">Oto niektóre z atrybutów adnotacji danych, które są używane do dodawania adnotacji do modelu albumu:</span><span class="sxs-lookup"><span data-stu-id="03e4a-470">These are some of the Data Annotation attributes you are using to annotate the Album model:</span></span>
    > 
    > - <span data-ttu-id="03e4a-471">Wymagane — wskazuje, że właściwość jest polem wymaganym</span><span class="sxs-lookup"><span data-stu-id="03e4a-471">Required - Indicates that the property is a required field</span></span>
    > - <span data-ttu-id="03e4a-472">DisplayName — definiuje tekst, który ma być używany dla pól formularza i komunikatów weryfikacji</span><span class="sxs-lookup"><span data-stu-id="03e4a-472">DisplayName - Defines the text to be used on form fields and validation messages</span></span>
    > - <span data-ttu-id="03e4a-473">DisplayFormat — określa sposób wyświetlania i formatowania pól danych.</span><span class="sxs-lookup"><span data-stu-id="03e4a-473">DisplayFormat - Specifies how data fields are displayed and formatted.</span></span>
    > - <span data-ttu-id="03e4a-474">StringLength — definiuje maksymalną długość pola ciągu</span><span class="sxs-lookup"><span data-stu-id="03e4a-474">StringLength - Defines a maximum length for a string field</span></span>
    > - <span data-ttu-id="03e4a-475">Zakres — daje maksymalną i minimalną wartość pola liczbowego</span><span class="sxs-lookup"><span data-stu-id="03e4a-475">Range - Gives a maximum and minimum value for a numeric field</span></span>
    > - <span data-ttu-id="03e4a-476">ScaffoldColumn — umożliwia ukrywanie pól z formularzy edytora</span><span class="sxs-lookup"><span data-stu-id="03e4a-476">ScaffoldColumn - Allows hiding fields from editor forms</span></span>

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="03e4a-477">Zadanie 2 — Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="03e4a-477">Task 2 - Running the Application</span></span>

<span data-ttu-id="03e4a-478">W tym zadaniu sprawdzisz, czy pola Utwórz i edytuj weryfikują strony przy użyciu nazw wyświetlanych wybranych w ostatnim zadaniu.</span><span class="sxs-lookup"><span data-stu-id="03e4a-478">In this task, you will test that the Create and Edit pages validate fields, using the display names chosen in the last task.</span></span>

1. <span data-ttu-id="03e4a-479">Naciśnij klawisz **F5** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="03e4a-479">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="03e4a-480">Projekt zostanie uruchomiony na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="03e4a-480">The project starts in the Home page.</span></span> <span data-ttu-id="03e4a-481">Zmień adres URL na **/StoreManager/Create**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-481">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="03e4a-482">Sprawdź, czy nazwy wyświetlane są zgodne z tymi w klasie częściowej (na przykład **adres URL grafiki albumu** zamiast **AlbumArtUrl**)</span><span class="sxs-lookup"><span data-stu-id="03e4a-482">Verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**)</span></span>
3. <span data-ttu-id="03e4a-483">Kliknij przycisk **Utwórz**, bez wypełniania formularza.</span><span class="sxs-lookup"><span data-stu-id="03e4a-483">Click **Create**, without filling the form.</span></span> <span data-ttu-id="03e4a-484">Sprawdź, czy są wyświetlone odpowiednie komunikaty weryfikacyjne.</span><span class="sxs-lookup"><span data-stu-id="03e4a-484">Verify that you get the corresponding validation messages.</span></span>

    <span data-ttu-id="03e4a-485">![Zweryfikowane pola na stronie tworzenia](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "Zweryfikowane pola na stronie tworzenia")</span><span class="sxs-lookup"><span data-stu-id="03e4a-485">![Validated fields in the Create page](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "Validated fields in the Create page")</span></span>

    <span data-ttu-id="03e4a-486">*Zweryfikowane pola na stronie tworzenia*</span><span class="sxs-lookup"><span data-stu-id="03e4a-486">*Validated fields in the Create page*</span></span>
4. <span data-ttu-id="03e4a-487">Możesz sprawdzić, czy na stronie **edytowania** jest to samo.</span><span class="sxs-lookup"><span data-stu-id="03e4a-487">You can verify that the same occurs with the **Edit** page.</span></span> <span data-ttu-id="03e4a-488">Zmień adres URL na **/StoreManager/Edit/1** i upewnij się, że nazwy wyświetlane są zgodne z tymi w klasie częściowej (na przykład **adres URL grafiki albumu** zamiast **AlbumArtUrl**).</span><span class="sxs-lookup"><span data-stu-id="03e4a-488">Change the URL to **/StoreManager/Edit/1** and verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**).</span></span> <span data-ttu-id="03e4a-489">Opróżnij pola **tytuł** i **Cena** , a następnie kliknij przycisk **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-489">Empty the **Title** and **Price** fields and click **Save**.</span></span> <span data-ttu-id="03e4a-490">Sprawdź, czy są wyświetlone odpowiednie komunikaty weryfikacyjne.</span><span class="sxs-lookup"><span data-stu-id="03e4a-490">Verify that you get the corresponding validation messages.</span></span>

    ![Zweryfikowane pola na stronie edytowania](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    <span data-ttu-id="03e4a-492">*Zweryfikowane pola na stronie edytowania*</span><span class="sxs-lookup"><span data-stu-id="03e4a-492">*Validated fields in the Edit page*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a><span data-ttu-id="03e4a-493">Ćwiczenie 7: korzystanie z niezauważalnej jQuery na stronie klienta</span><span class="sxs-lookup"><span data-stu-id="03e4a-493">Exercise 7: Using Unobtrusive jQuery at Client Side</span></span>

<span data-ttu-id="03e4a-494">W tym ćwiczeniu dowiesz się, jak włączyć sprawdzanie poprawności jQuery na platformie MVC 4 po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="03e4a-494">In this exercise, you will learn how to enable MVC 4 Unobtrusive jQuery validation at client side.</span></span>

> [!NOTE]
> <span data-ttu-id="03e4a-495">Niewygodna jQuery używa prefiksu danych-AJAX języka JavaScript do wywoływania metod akcji na serwerze, a nie w sposób niezauważalny emituje wbudowane skrypty klienta.</span><span class="sxs-lookup"><span data-stu-id="03e4a-495">The Unobtrusive jQuery uses data-ajax prefix JavaScript to invoke action methods on the server rather than intrusively emitting inline client scripts.</span></span>

<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a><span data-ttu-id="03e4a-496">Zadanie 1 — Uruchamianie aplikacji przed włączeniem dyskretnej jQuery</span><span class="sxs-lookup"><span data-stu-id="03e4a-496">Task 1 - Running the Application before Enabling Unobtrusive jQuery</span></span>

<span data-ttu-id="03e4a-497">W tym zadaniu zostanie uruchomiona aplikacja przed dołączeniem jQuery w celu porównania obu modeli walidacji.</span><span class="sxs-lookup"><span data-stu-id="03e4a-497">In this task, you will run the application before including jQuery in order to compare both validation models.</span></span>

1. <span data-ttu-id="03e4a-498">Otwórz rozwiązanie **BEGIN** znajdujące się w lokalizacji **Source/Ex7-UnobtrusivejQueryValidation/BEGIN/** folder.</span><span class="sxs-lookup"><span data-stu-id="03e4a-498">Open the **Begin** solution located at **Source/Ex7-UnobtrusivejQueryValidation/Begin/** folder.</span></span> <span data-ttu-id="03e4a-499">W przeciwnym razie można nadal korzystać z rozwiązania **końcowego** uzyskanego przez zakończenie poprzedniego ćwiczenia.</span><span class="sxs-lookup"><span data-stu-id="03e4a-499">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="03e4a-500">Jeśli otwarto **udostępnione rozwiązanie** , należy pobrać brakujące pakiety NuGet przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="03e4a-500">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="03e4a-501">W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-501">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="03e4a-502">W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.</span><span class="sxs-lookup"><span data-stu-id="03e4a-502">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="03e4a-503">Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-503">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="03e4a-504">Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="03e4a-504">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="03e4a-505">Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu.</span><span class="sxs-lookup"><span data-stu-id="03e4a-505">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="03e4a-506">To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="03e4a-506">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="03e4a-507">Naciśnij klawisz **F5**, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="03e4a-507">Press **F5** to run the application.</span></span>
3. <span data-ttu-id="03e4a-508">Projekt zostanie uruchomiony na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="03e4a-508">The project starts in the Home page.</span></span> <span data-ttu-id="03e4a-509">Przeglądaj **/StoreManager/Create** i kliknij przycisk **Utwórz** bez wypełniania formularza, aby sprawdzić, czy są odbierane komunikaty weryfikacji:</span><span class="sxs-lookup"><span data-stu-id="03e4a-509">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="03e4a-510">![Weryfikacja klienta wyłączona](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "Weryfikacja klienta wyłączona")</span><span class="sxs-lookup"><span data-stu-id="03e4a-510">![Client validation disabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "Client validation disabled")</span></span>

    <span data-ttu-id="03e4a-511">*Weryfikacja klienta wyłączona*</span><span class="sxs-lookup"><span data-stu-id="03e4a-511">*Client validation disabled*</span></span>
4. <span data-ttu-id="03e4a-512">W przeglądarce Otwórz kod źródłowy HTML:</span><span class="sxs-lookup"><span data-stu-id="03e4a-512">In the browser, open the HTML source code:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a><span data-ttu-id="03e4a-513">Zadanie 2 — Włączanie niezauważalnej weryfikacji klienta</span><span class="sxs-lookup"><span data-stu-id="03e4a-513">Task 2 - Enabling Unobtrusive Client Validation</span></span>

<span data-ttu-id="03e4a-514">W tym zadaniu zostanie włączone **niezauważalne sprawdzanie poprawności klienta** w usłudze jQuery z pliku **Web. config** , który domyślnie ma wartość false we wszystkich nowych projektach ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="03e4a-514">In this task, you will enable jQuery **unobtrusive client validation** from **Web.config** file, which is by default set to false in all new ASP.NET MVC 4 projects.</span></span> <span data-ttu-id="03e4a-515">Ponadto dodasz niezbędne odwołania do skryptów, aby umożliwić niewygodną weryfikację klienta w technologii jQuery.</span><span class="sxs-lookup"><span data-stu-id="03e4a-515">Additionally, you will add the necessary scripts references to make jQuery Unobtrusive Client Validation work.</span></span>

1. <span data-ttu-id="03e4a-516">Otwórz plik **Web. config** w katalogu głównym projektu i upewnij się, że wartości kluczy **ClientValidationEnabled** i **UnobtrusiveJavaScriptEnabled** są ustawione na **wartość true**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-516">Open **Web.Config** file at project root, and make sure that the **ClientValidationEnabled** and **UnobtrusiveJavaScriptEnabled** keys values are set to **true**.</span></span>

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > <span data-ttu-id="03e4a-517">Możesz również włączyć sprawdzanie poprawności klienta przez kod w Global.asax.cs, aby uzyskać te same wyniki:</span><span class="sxs-lookup"><span data-stu-id="03e4a-517">You can also enable client validation by code at Global.asax.cs to get the same results:</span></span>
    > 
    > <span data-ttu-id="03e4a-518">**HtmlHelper. ClientValidationEnabled = true;**</span><span class="sxs-lookup"><span data-stu-id="03e4a-518">**HtmlHelper.ClientValidationEnabled = true;**</span></span>
    > 
    > <span data-ttu-id="03e4a-519">Ponadto można przypisać atrybut ClientValidationEnabled do dowolnego kontrolera, aby mieć niestandardowe zachowanie.</span><span class="sxs-lookup"><span data-stu-id="03e4a-519">Additionally, you can assign ClientValidationEnabled attribute into any controller to have a custom behavior.</span></span>
2. <span data-ttu-id="03e4a-520">Otwórz **Create. cshtml** w **Views\StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-520">Open **Create.cshtml** at **Views\StoreManager**.</span></span>
3. <span data-ttu-id="03e4a-521">Upewnij się, że następujące pliki skryptów, **jQuery. Validate** i **jQuery. Validate**, są przywoływane w widoku za pomocą pakietu &quot; **~/bundles/jqueryval**&quot;.</span><span class="sxs-lookup"><span data-stu-id="03e4a-521">Make sure the following script files, **jquery.validate** and **jquery.validate.unobtrusive**, are referenced in the view through the &quot;**~/bundles/jqueryval**&quot; bundle.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="03e4a-522">Wszystkie te biblioteki jQuery są zawarte w nowych projektach MVC 4.</span><span class="sxs-lookup"><span data-stu-id="03e4a-522">All these jQuery libraries are included in MVC 4 new projects.</span></span> <span data-ttu-id="03e4a-523">Więcej bibliotek można znaleźć w folderze **/scripts** projektu.</span><span class="sxs-lookup"><span data-stu-id="03e4a-523">You can find more libraries in the **/Scripts** folder of you project.</span></span>
    > 
    > <span data-ttu-id="03e4a-524">Aby te biblioteki sprawdzania poprawności działały, należy dodać odwołanie do biblioteki platformy jQuery.</span><span class="sxs-lookup"><span data-stu-id="03e4a-524">In order to make this validation libraries work, you need to add a reference to the jQuery framework library.</span></span> <span data-ttu-id="03e4a-525">Ponieważ to odwołanie zostało już dodane w pliku **\_Layout. cshtml** , nie trzeba go dodawać w tym konkretnym widoku.</span><span class="sxs-lookup"><span data-stu-id="03e4a-525">Since this reference is already added in the **\_Layout.cshtml** file, you do not need to add it in this particular view.</span></span>

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a><span data-ttu-id="03e4a-526">Zadanie 3 — Uruchamianie aplikacji przy użyciu niezauważalnej weryfikacji jQuery</span><span class="sxs-lookup"><span data-stu-id="03e4a-526">Task 3 - Running the Application Using Unobtrusive jQuery Validation</span></span>

<span data-ttu-id="03e4a-527">W tym zadaniu zostanie przetestowana, że szablon Tworzenie widoku programu **storemanager** wykonuje weryfikację po stronie klienta przy użyciu bibliotek jQuery, gdy użytkownik tworzy nowy album.</span><span class="sxs-lookup"><span data-stu-id="03e4a-527">In this task, you will test that the **StoreManager** create view template performs client side validation using jQuery libraries when the user creates a new album.</span></span>

1. <span data-ttu-id="03e4a-528">Naciśnij klawisz **F5**, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="03e4a-528">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="03e4a-529">Projekt zostanie uruchomiony na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="03e4a-529">The project starts in the Home page.</span></span> <span data-ttu-id="03e4a-530">Przeglądaj **/StoreManager/Create** i kliknij przycisk **Utwórz** bez wypełniania formularza, aby sprawdzić, czy są odbierane komunikaty weryfikacji:</span><span class="sxs-lookup"><span data-stu-id="03e4a-530">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="03e4a-531">![Sprawdzanie poprawności klienta przy włączonej platformie jQuery](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "Sprawdzanie poprawności klienta przy włączonej platformie jQuery")</span><span class="sxs-lookup"><span data-stu-id="03e4a-531">![Client validation with jQuery enabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "Client validation with jQuery enabled")</span></span>

    <span data-ttu-id="03e4a-532">*Sprawdzanie poprawności klienta przy włączonej platformie jQuery*</span><span class="sxs-lookup"><span data-stu-id="03e4a-532">*Client validation with jQuery enabled*</span></span>
3. <span data-ttu-id="03e4a-533">W przeglądarce Otwórz kod źródłowy dla tworzenia widoku:</span><span class="sxs-lookup"><span data-stu-id="03e4a-533">In the browser, open the source code for Create view:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

   > [!NOTE]
   > <span data-ttu-id="03e4a-534">Dla każdej reguły walidacji klienta niezauważalne jQuery dodaje atrybut z danymi-Val-*rulename*=&quot;*komunikatów*&quot;.</span><span class="sxs-lookup"><span data-stu-id="03e4a-534">For each client validation rule, Unobtrusive jQuery adds an attribute with data-val-*rulename*=&quot;*message*&quot;.</span></span> <span data-ttu-id="03e4a-535">Poniżej znajduje się lista tagów niezauważalnych przez jQuery wstawianych do pola wejściowego HTML w celu przeprowadzenia weryfikacji klienta:</span><span class="sxs-lookup"><span data-stu-id="03e4a-535">Below is a list of tags that Unobtrusive jQuery inserts into the html input field to perform client validation:</span></span>
   > 
   > - <span data-ttu-id="03e4a-536">Dane — Val</span><span class="sxs-lookup"><span data-stu-id="03e4a-536">Data-val</span></span>
   > - <span data-ttu-id="03e4a-537">Data-Val-Number</span><span class="sxs-lookup"><span data-stu-id="03e4a-537">Data-val-number</span></span>
   > - <span data-ttu-id="03e4a-538">Data-Val-Range</span><span class="sxs-lookup"><span data-stu-id="03e4a-538">Data-val-range</span></span>
   > - <span data-ttu-id="03e4a-539">Data-Val-Range-min/Data-Val-Range-Max</span><span class="sxs-lookup"><span data-stu-id="03e4a-539">Data-val-range-min / Data-val-range-max</span></span>
   > - <span data-ttu-id="03e4a-540">Data-Val — wymagane</span><span class="sxs-lookup"><span data-stu-id="03e4a-540">Data-val-required</span></span>
   > - <span data-ttu-id="03e4a-541">Data-Val — Długość</span><span class="sxs-lookup"><span data-stu-id="03e4a-541">Data-val-length</span></span>
   > - <span data-ttu-id="03e4a-542">Data-Val-Length-Max/Data-Val-Length — min</span><span class="sxs-lookup"><span data-stu-id="03e4a-542">Data-val-length-max / Data-val-length-min</span></span>
   > 
   > <span data-ttu-id="03e4a-543">Wszystkie wartości danych są wypełnione **adnotacjami danych**modelu.</span><span class="sxs-lookup"><span data-stu-id="03e4a-543">All the data values are filled with model **Data Annotation**.</span></span> <span data-ttu-id="03e4a-544">Następnie Cała logika, która działa po stronie serwera, może być uruchamiana po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="03e4a-544">Then, all the logic that works at server side can be run at client side.</span></span> <span data-ttu-id="03e4a-545">Na przykład atrybut Price ma następującą adnotację danych w modelu:</span><span class="sxs-lookup"><span data-stu-id="03e4a-545">For example, Price attribute has the following data annotation in the model:</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
   > 
   > <span data-ttu-id="03e4a-546">Po użyciu dyskretnej technologii jQuery wygenerowany kod jest:</span><span class="sxs-lookup"><span data-stu-id="03e4a-546">After using Unobtrusive jQuery, the generated code is:</span></span>
   > 
   > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="03e4a-547">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="03e4a-547">Summary</span></span>

<span data-ttu-id="03e4a-548">Dzięki zakończeniu tego praktycznego laboratorium wiesz już, jak umożliwić użytkownikom zmianę danych przechowywanych w bazie danych przy użyciu następujących elementów:</span><span class="sxs-lookup"><span data-stu-id="03e4a-548">By completing this Hands-On Lab you have learned how to enable users to change the data stored in the database with the use of the following:</span></span>

- <span data-ttu-id="03e4a-549">Akcje kontrolera, takie jak indeks, tworzenie, edytowanie, usuwanie</span><span class="sxs-lookup"><span data-stu-id="03e4a-549">Controller actions like Index, Create, Edit, Delete</span></span>
- <span data-ttu-id="03e4a-550">Funkcja szkieletu ASP.NET MVC do wyświetlania właściwości w tabeli HTML</span><span class="sxs-lookup"><span data-stu-id="03e4a-550">ASP.NET MVC's scaffolding feature for displaying properties in an HTML table</span></span>
- <span data-ttu-id="03e4a-551">Niestandardowi pomocnicy HTML do ulepszania środowiska użytkownika</span><span class="sxs-lookup"><span data-stu-id="03e4a-551">Custom HTML helpers to improve user experience</span></span>
- <span data-ttu-id="03e4a-552">Metody akcji, które reagują na wywołania HTTP-GET lub HTTP-POST</span><span class="sxs-lookup"><span data-stu-id="03e4a-552">Action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="03e4a-553">Szablon edytora udostępnionego dla podobnych szablonów widoku, takich jak tworzenie i edytowanie</span><span class="sxs-lookup"><span data-stu-id="03e4a-553">A shared editor template for similar View templates like Create and Edit</span></span>
- <span data-ttu-id="03e4a-554">Elementy formularza, takie jak listy rozwijane</span><span class="sxs-lookup"><span data-stu-id="03e4a-554">Form elements like drop-downs</span></span>
- <span data-ttu-id="03e4a-555">Adnotacje danych na potrzeby walidacji modelu</span><span class="sxs-lookup"><span data-stu-id="03e4a-555">Data annotations for Model validation</span></span>
- <span data-ttu-id="03e4a-556">Sprawdzanie poprawności po stronie klienta przy użyciu niedyskretnej biblioteki jQuery</span><span class="sxs-lookup"><span data-stu-id="03e4a-556">Client Side Validation using jQuery Unobtrusive library</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="03e4a-557">Dodatek A: Instalowanie Visual Studio Express 2012 dla sieci Web</span><span class="sxs-lookup"><span data-stu-id="03e4a-557">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="03e4a-558">Można zainstalować **Microsoft Visual Studio Express 2012 dla sieci Web** lub innej &quot;Express&quot; wersji za pomocą **[Instalator platformy Microsoft Web](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="03e4a-558">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="03e4a-559">Poniższe instrukcje przeprowadzą Cię przez kroki wymagane do zainstalowania *programu Visual Studio Express 2012 for Web* przy użyciu *Instalator platformy Microsoft Web*.</span><span class="sxs-lookup"><span data-stu-id="03e4a-559">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="03e4a-560">Przejdź do [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="03e4a-560">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="03e4a-561">Alternatywnie, jeśli zainstalowano już Instalatora platformy sieci Web, można go otworzyć i wyszukać produkt &quot;<em>Visual Studio Express 2012 for Web przy użyciu zestawu Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="03e4a-561">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="03e4a-562">Kliknij pozycję **Zainstaluj teraz**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-562">Click on **Install Now**.</span></span> <span data-ttu-id="03e4a-563">Jeśli nie masz **Instalatora platformy sieci Web** , nastąpi przekierowanie do pobrania i zainstalowania go w pierwszej kolejności.</span><span class="sxs-lookup"><span data-stu-id="03e4a-563">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="03e4a-564">Po otwarciu **Instalatora platformy sieci Web** kliknij przycisk **Zainstaluj** , aby rozpocząć instalację.</span><span class="sxs-lookup"><span data-stu-id="03e4a-564">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="03e4a-565">![Zainstaluj Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Zainstaluj Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="03e4a-565">![Install Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="03e4a-566">*Zainstaluj Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="03e4a-566">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="03e4a-567">Przeczytaj wszystkie licencje i postanowienia produktów, a następnie kliknij przycisk **Akceptuję** , aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="03e4a-567">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akceptowanie postanowień licencyjnych](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    <span data-ttu-id="03e4a-569">*Akceptowanie postanowień licencyjnych*</span><span class="sxs-lookup"><span data-stu-id="03e4a-569">*Accepting the license terms*</span></span>
5. <span data-ttu-id="03e4a-570">Poczekaj na zakończenie procesu pobierania i instalacji.</span><span class="sxs-lookup"><span data-stu-id="03e4a-570">Wait until the downloading and installation process completes.</span></span>

    ![Postęp instalacji](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    <span data-ttu-id="03e4a-572">*Postęp instalacji*</span><span class="sxs-lookup"><span data-stu-id="03e4a-572">*Installation progress*</span></span>
6. <span data-ttu-id="03e4a-573">Po zakończeniu instalacji kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-573">When the installation completes, click **Finish**.</span></span>

    ![Instalacja zakończona](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    <span data-ttu-id="03e4a-575">*Instalacja zakończona*</span><span class="sxs-lookup"><span data-stu-id="03e4a-575">*Installation completed*</span></span>
7. <span data-ttu-id="03e4a-576">Kliknij przycisk **Zakończ** , aby zamknąć Instalatora platformy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="03e4a-576">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="03e4a-577">Aby otworzyć Visual Studio Express dla sieci Web, przejdź do ekranu **startowego** i zacznij pisać &quot;**vs Express**&quot;, a następnie kliknij kafelek **vs Express for Web** .</span><span class="sxs-lookup"><span data-stu-id="03e4a-577">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Kafelek VS Express for Web](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    <span data-ttu-id="03e4a-579">*Kafelek VS Express for Web*</span><span class="sxs-lookup"><span data-stu-id="03e4a-579">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="03e4a-580">Dodatek B: używanie fragmentów kodu</span><span class="sxs-lookup"><span data-stu-id="03e4a-580">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="03e4a-581">Za pomocą fragmentów kodu masz cały kod, którego potrzebujesz na wyręką.</span><span class="sxs-lookup"><span data-stu-id="03e4a-581">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="03e4a-582">Dokument laboratoryjny powiedzie się dokładnie wtedy, gdy można z nich korzystać, jak pokazano na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="03e4a-582">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="03e4a-583">![Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio")</span><span class="sxs-lookup"><span data-stu-id="03e4a-583">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="03e4a-584">*Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="03e4a-584">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="03e4a-585">***Aby dodać fragment kodu przy użyciu klawiatury (C# tylko)***</span><span class="sxs-lookup"><span data-stu-id="03e4a-585">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="03e4a-586">Umieść kursor, w którym chcesz wstawić kod.</span><span class="sxs-lookup"><span data-stu-id="03e4a-586">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="03e4a-587">Zacznij wpisywać nazwę fragmentu kodu (bez spacji lub łączników).</span><span class="sxs-lookup"><span data-stu-id="03e4a-587">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="03e4a-588">Obejrzyj jako IntelliSense wyświetla pasujące nazwy fragmentów kodu.</span><span class="sxs-lookup"><span data-stu-id="03e4a-588">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="03e4a-589">Wybierz poprawny fragment kodu (lub Kontynuuj wpisywanie, dopóki nie zostanie zaznaczona cała nazwa fragmentu kodu).</span><span class="sxs-lookup"><span data-stu-id="03e4a-589">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="03e4a-590">Naciśnij dwukrotnie klawisz Tab, aby wstawić fragment kodu w lokalizacji kursora.</span><span class="sxs-lookup"><span data-stu-id="03e4a-590">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="03e4a-591">![Zacznij wpisywać nazwę fragmentu kodu](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "Zacznij wpisywać nazwę fragmentu kodu")</span><span class="sxs-lookup"><span data-stu-id="03e4a-591">![Start typing the snippet name](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="03e4a-592">*Zacznij wpisywać nazwę fragmentu kodu*</span><span class="sxs-lookup"><span data-stu-id="03e4a-592">*Start typing the snippet name*</span></span>

<span data-ttu-id="03e4a-593">![Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu")</span><span class="sxs-lookup"><span data-stu-id="03e4a-593">![Press Tab to select the highlighted snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="03e4a-594">*Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="03e4a-594">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="03e4a-595">![Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta")</span><span class="sxs-lookup"><span data-stu-id="03e4a-595">![Press Tab again and the snippet will expand](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="03e4a-596">*Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta*</span><span class="sxs-lookup"><span data-stu-id="03e4a-596">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="03e4a-597">***Aby dodać fragment kodu przy użyciu myszy (C#, Visual Basic i XML)*** jedno.</span><span class="sxs-lookup"><span data-stu-id="03e4a-597">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="03e4a-598">Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu.</span><span class="sxs-lookup"><span data-stu-id="03e4a-598">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="03e4a-599">Wybierz **Wstaw fragment** kodu, po którym następuje **Moje fragmenty kodów**.</span><span class="sxs-lookup"><span data-stu-id="03e4a-599">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="03e4a-600">Wybierz odpowiedni fragment kodu z listy, klikając go.</span><span class="sxs-lookup"><span data-stu-id="03e4a-600">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="03e4a-601">![Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment")</span><span class="sxs-lookup"><span data-stu-id="03e4a-601">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="03e4a-602">*Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment*</span><span class="sxs-lookup"><span data-stu-id="03e4a-602">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="03e4a-603">![Wybierz odpowiedni fragment kodu z listy, klikając go](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "Wybierz odpowiedni fragment kodu z listy, klikając go")</span><span class="sxs-lookup"><span data-stu-id="03e4a-603">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="03e4a-604">*Wybierz odpowiedni fragment kodu z listy, klikając go*</span><span class="sxs-lookup"><span data-stu-id="03e4a-604">*Pick the relevant snippet from the list, by clicking on it*</span></span>
