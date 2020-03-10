---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: ASP.NET modelu MVC 4 i dostęp do danych | Microsoft Docs
author: rick-anderson
description: 'Uwaga: w tym ćwiczeniu praktycznym założono, że masz podstawową wiedzę na temat ASP.NET MVC. Jeśli przed nie korzystasz z ASP.NET MVC, zalecamy przejście do ASP.NET MVC 4...'
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: 90635b617930d0a9c126795f4c8790d542e33dc9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78560201"
---
# <a name="aspnet-mvc-4-models-and-data-access"></a><span data-ttu-id="be9d3-104">ASP.NET MVC 4 — modele i dostęp do danych</span><span class="sxs-lookup"><span data-stu-id="be9d3-104">ASP.NET MVC 4 Models and Data Access</span></span>

<span data-ttu-id="be9d3-105">przez [zespół Camp sieci Web](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="be9d3-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="be9d3-106">Pobierz zestaw szkoleniowy dla sieci Web Camp</span><span class="sxs-lookup"><span data-stu-id="be9d3-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="be9d3-107">W ramach tego ćwiczenia praktycznego założono, że masz podstawową wiedzę na temat **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="be9d3-107">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="be9d3-108">Jeśli wcześniej nie korzystasz z **ASP.NET MVC** , zalecamy przechodzenie przez **ASP.NETe podstawowe wskazówki dotyczące MVC 4** .</span><span class="sxs-lookup"><span data-stu-id="be9d3-108">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>

<span data-ttu-id="be9d3-109">To laboratorium przeprowadzi Cię przez ulepszenia i nowe funkcje opisane wcześniej przez zastosowanie drobnych zmian do przykładowej aplikacji sieci Web, która znajduje się w folderze źródłowym.</span><span class="sxs-lookup"><span data-stu-id="be9d3-109">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>

> [!NOTE]
> <span data-ttu-id="be9d3-110">Wszystkie przykładowe kod i fragmenty kodu są zawarte w zestawie szkoleń w sieci Web Camp, które są dostępne w [wersjach Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="be9d3-110">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="be9d3-111">Projekt specyficzny dla tego laboratorium jest dostępny w [modelach ASP.NET MVC 4 i dostęp do danych](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).</span><span class="sxs-lookup"><span data-stu-id="be9d3-111">The project specific to this lab is available at [ASP.NET MVC 4 Models and Data Access](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).</span></span>

<span data-ttu-id="be9d3-112">W środowisku **ASP.NET MVC** — Omówienie praktycznego przekazywania danych z kontrolerów do szablonów widoków.</span><span class="sxs-lookup"><span data-stu-id="be9d3-112">In **ASP.NET MVC Fundamentals** Hands-on Lab, you have been passing hard-coded data from the Controllers to the View templates.</span></span> <span data-ttu-id="be9d3-113">Jednak w celu utworzenia rzeczywistej aplikacji sieci Web warto użyć rzeczywistej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="be9d3-113">But, in order to build a real Web application, you might want to use a real database.</span></span>

<span data-ttu-id="be9d3-114">W ramach tego praktycznego laboratorium pokazano, jak używać aparatu bazy danych w celu przechowywania i pobierania danych potrzebnych dla aplikacji do sklepu muzycznego.</span><span class="sxs-lookup"><span data-stu-id="be9d3-114">This Hands-on Lab will show you how to use a database engine in order to store and retrieve the data needed for the Music Store application.</span></span> <span data-ttu-id="be9d3-115">Aby to osiągnąć, należy rozpocząć od istniejącej bazy danych i utworzyć Entity Data Model od niej.</span><span class="sxs-lookup"><span data-stu-id="be9d3-115">To accomplish that, you will start with an existing database and create the Entity Data Model from it.</span></span> <span data-ttu-id="be9d3-116">W tym laboratorium spełnimy podejście **Database First** , a także podejście **Code First** .</span><span class="sxs-lookup"><span data-stu-id="be9d3-116">Throughout this lab, you will meet the **Database First** approach as well as the **Code First** approach.</span></span>

<span data-ttu-id="be9d3-117">Można jednak również użyć podejścia **model First** , utworzyć ten sam model przy użyciu narzędzi, a następnie wygenerować z niego bazę danych.</span><span class="sxs-lookup"><span data-stu-id="be9d3-117">However, you can also use the **Model First** approach, create the same model using the tools, and then generate the database from it.</span></span>

<span data-ttu-id="be9d3-118">![Database First a Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First a Model First")</span><span class="sxs-lookup"><span data-stu-id="be9d3-118">![Database First vs. Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First vs. Model First")</span></span>

<span data-ttu-id="be9d3-119">*Database First a Model First*</span><span class="sxs-lookup"><span data-stu-id="be9d3-119">*Database First vs. Model First*</span></span>

<span data-ttu-id="be9d3-120">Po wygenerowaniu modelu wprowadzisz odpowiednie korekty w StoreController, aby udostępnić widoki magazynu z danymi pobranymi z bazy danych, zamiast korzystać z zakodowanych danych.</span><span class="sxs-lookup"><span data-stu-id="be9d3-120">After generating the Model, you will make the proper adjustments in the StoreController to provide the Store Views with the data taken from the database, instead of using hard-coded data.</span></span> <span data-ttu-id="be9d3-121">Nie musisz wprowadzać żadnych zmian w szablonach widoku, ponieważ StoreController będzie zwracać ten sam modele widoków do szablonów widoków, chociaż dane będą pochodzić z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="be9d3-121">You will not need to make any change to the View templates because the StoreController will be returning the same ViewModels to the View templates, although this time the data will come from the database.</span></span>

<span data-ttu-id="be9d3-122">**Podejście Code First**</span><span class="sxs-lookup"><span data-stu-id="be9d3-122">**The Code First Approach**</span></span>

<span data-ttu-id="be9d3-123">Podejście Code First umożliwia zdefiniowanie modelu na podstawie kodu bez generowania klas, które są ogólnie powiązane z platformą.</span><span class="sxs-lookup"><span data-stu-id="be9d3-123">The Code First approach allows us to define the model from the code without generating classes that are generally coupled with the framework.</span></span>

<span data-ttu-id="be9d3-124">W kodzie najpierw obiekty modelu są definiowane za pomocą POCOs, &quot;zwykłych starych obiektów CLR&quot;.</span><span class="sxs-lookup"><span data-stu-id="be9d3-124">In code first, model objects are defined with POCOs, &quot;Plain Old CLR Objects&quot;.</span></span> <span data-ttu-id="be9d3-125">POCOs są prostymi klasami czystymi, które nie mają dziedziczenia i nie implementują interfejsów.</span><span class="sxs-lookup"><span data-stu-id="be9d3-125">POCOs are simple plain classes that have no inheritance and do not implement interfaces.</span></span> <span data-ttu-id="be9d3-126">Można automatycznie wygenerować z nich bazę danych lub użyć istniejącej bazy danych i wygenerować mapowanie klasy na podstawie kodu.</span><span class="sxs-lookup"><span data-stu-id="be9d3-126">We can automatically generate the database from them, or we can use an existing database and generate the class mapping from the code.</span></span>

<span data-ttu-id="be9d3-127">Zaletą tego podejścia jest to, że model pozostaje niezależny od struktury trwałości (w tym przypadku Entity Framework), ponieważ klasy POCOs nie są sprzężone z strukturą mapowania.</span><span class="sxs-lookup"><span data-stu-id="be9d3-127">The benefits of using this approach is that the Model remains independent from the persistence framework (in this case, Entity Framework), as the POCOs classes are not coupled with the mapping framework.</span></span>

> [!NOTE]
> <span data-ttu-id="be9d3-128">To laboratorium jest oparte na ASP.NET MVC 4 oraz wersji przykładowej aplikacji, która została dostosowana i zminimalizowana do dopasowania tylko do funkcji przedstawionych w tym środowisku.</span><span class="sxs-lookup"><span data-stu-id="be9d3-128">This Lab is based on ASP.NET MVC 4 and a version of the Music Store sample application customized and minimized to fit only the features shown in this Hands-On Lab.</span></span>
> 
> <span data-ttu-id="be9d3-129">Jeśli chcesz zapoznać się z samouczkiem dotyczącym całego **sklepu muzycznego** , możesz go znaleźć w [sklepie MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="be9d3-129">If you wish to explore the whole **Music Store** tutorial application you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="be9d3-130">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="be9d3-130">Prerequisites</span></span>

<span data-ttu-id="be9d3-131">Aby ukończyć to laboratorium, musisz mieć następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="be9d3-131">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="be9d3-132">[Microsoft Visual Studio Express 2012 dla sieci Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub nadrzędnych (Przeczytaj [dodatek A,](#AppendixA) Aby uzyskać instrukcje na temat sposobu jego instalacji).</span><span class="sxs-lookup"><span data-stu-id="be9d3-132">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="be9d3-133">Konfigurowanie</span><span class="sxs-lookup"><span data-stu-id="be9d3-133">Setup</span></span>

<span data-ttu-id="be9d3-134">**Instalowanie fragmentów kodu**</span><span class="sxs-lookup"><span data-stu-id="be9d3-134">**Installing Code Snippets**</span></span>

<span data-ttu-id="be9d3-135">Dla wygody większość kodu, który będzie zarządzany w tym laboratorium, jest dostępna jako fragmenty kodu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="be9d3-135">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="be9d3-136">Aby zainstalować plik fragmentów kodu, uruchom **.\Source\Setup\CodeSnippets.VSI** .</span><span class="sxs-lookup"><span data-stu-id="be9d3-136">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="be9d3-137">Jeśli nie znasz fragmentów kodu Visual Studio Code i chcesz dowiedzieć się, jak z nich korzystać, możesz odwołać się do dodatku z tego dokumentu &quot;[dodatku C: Using fragmenty kodu](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="be9d3-137">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="be9d3-138">Symulacyjn</span><span class="sxs-lookup"><span data-stu-id="be9d3-138">Exercises</span></span>

<span data-ttu-id="be9d3-139">To laboratorium praktyczne obejmuje następujące ćwiczenia:</span><span class="sxs-lookup"><span data-stu-id="be9d3-139">This Hands-on Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="be9d3-140">Ćwiczenie 1: Dodawanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="be9d3-140">Exercise 1: Adding a Database</span></span>](#Exercise1)
2. [<span data-ttu-id="be9d3-141">Ćwiczenie 2: Tworzenie bazy danych przy użyciu Code First</span><span class="sxs-lookup"><span data-stu-id="be9d3-141">Exercise 2: Creating a Database using Code First</span></span>](#Exercise2)
3. [<span data-ttu-id="be9d3-142">Ćwiczenie 3: wykonywanie zapytania dotyczącego bazy danych za pomocą parametrów</span><span class="sxs-lookup"><span data-stu-id="be9d3-142">Exercise 3: Querying the Database with Parameters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="be9d3-143">Każdemu ćwiczeniu towarzyszy folder **końcowy** zawierający rozwiązanie, które należy uzyskać po zakończeniu ćwiczenia.</span><span class="sxs-lookup"><span data-stu-id="be9d3-143">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="be9d3-144">Tego rozwiązania można użyć jako przewodnika, jeśli potrzebujesz dodatkowej pomocy w pracy przez ćwiczenia.</span><span class="sxs-lookup"><span data-stu-id="be9d3-144">You can use this solution as a guide if you need additional help working through the exercises.</span></span>

<span data-ttu-id="be9d3-145">Szacowany czas wykonywania tego laboratorium: **35 minut**.</span><span class="sxs-lookup"><span data-stu-id="be9d3-145">Estimated time to complete this lab: **35 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a><span data-ttu-id="be9d3-146">Ćwiczenie 1: Dodawanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="be9d3-146">Exercise 1: Adding a Database</span></span>

<span data-ttu-id="be9d3-147">W tym ćwiczeniu dowiesz się, jak dodać bazę danych z tabelami aplikacji MusicStore do rozwiązania, aby móc korzystać z jego danych.</span><span class="sxs-lookup"><span data-stu-id="be9d3-147">In this exercise, you will learn how to add a database with the tables of the MusicStore application to the solution in order to consume its data.</span></span> <span data-ttu-id="be9d3-148">Gdy baza danych jest generowana przy użyciu modelu i dodawana do rozwiązania, należy zmodyfikować klasę StoreController, aby udostępnić szablon widoku z danymi pobranymi z bazy danych, zamiast korzystać z zakodowanych wartości.</span><span class="sxs-lookup"><span data-stu-id="be9d3-148">Once the database is generated with the model, and added to the solution, you will modify the StoreController class to provide the View template with the data taken from the database, instead of using hard-coded values.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a><span data-ttu-id="be9d3-149">Zadanie 1 — Dodawanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="be9d3-149">Task 1 - Adding a Database</span></span>

<span data-ttu-id="be9d3-150">W tym zadaniu dodasz już utworzoną bazę danych z głównymi tabelami aplikacji MusicStore do rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="be9d3-150">In this task, you will add an already created database with the main tables of the MusicStore application to the solution.</span></span>

1. <span data-ttu-id="be9d3-151">Otwórz rozwiązanie **BEGIN** znajdujące się w lokalizacji **Source/Ex1-AddingADatabaseDBFirst/BEGIN/** folder.</span><span class="sxs-lookup"><span data-stu-id="be9d3-151">Open the **Begin** solution located at **Source/Ex1-AddingADatabaseDBFirst/Begin/** folder.</span></span>

   1. <span data-ttu-id="be9d3-152">Przed kontynuowaniem musisz pobrać brakujące pakiety NuGet.</span><span class="sxs-lookup"><span data-stu-id="be9d3-152">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="be9d3-153">W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="be9d3-153">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="be9d3-154">W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.</span><span class="sxs-lookup"><span data-stu-id="be9d3-154">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="be9d3-155">Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="be9d3-155">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="be9d3-156">Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="be9d3-156">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="be9d3-157">Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu.</span><span class="sxs-lookup"><span data-stu-id="be9d3-157">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="be9d3-158">To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="be9d3-158">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="be9d3-159">Dodaj plik bazy danych **MvcMusicStore** .</span><span class="sxs-lookup"><span data-stu-id="be9d3-159">Add **MvcMusicStore** database file.</span></span> <span data-ttu-id="be9d3-160">W ramach tego praktycznego laboratorium zostanie użyta już utworzona baza danych o nazwie **MvcMusicStore. mdf**.</span><span class="sxs-lookup"><span data-stu-id="be9d3-160">In this Hands-on Lab, you will use an already created database called **MvcMusicStore.mdf**.</span></span> <span data-ttu-id="be9d3-161">Aby to zrobić, kliknij prawym przyciskiem myszy pozycję **aplikacja\_dane** , wskaż polecenie **Dodaj** , a następnie kliknij pozycję **istniejący element**.</span><span class="sxs-lookup"><span data-stu-id="be9d3-161">To do that, right-click **App\_Data** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="be9d3-162">Przejdź do **\Source\Assets** i wybierz plik **MvcMusicStore. mdf** .</span><span class="sxs-lookup"><span data-stu-id="be9d3-162">Browse to **\Source\Assets** and select the **MvcMusicStore.mdf** file.</span></span>

    <span data-ttu-id="be9d3-163">![Dodawanie istniejącego elementu](aspnet-mvc-4-models-and-data-access/_static/image2.png "Dodawanie istniejącego elementu")</span><span class="sxs-lookup"><span data-stu-id="be9d3-163">![Adding an Existing Item](aspnet-mvc-4-models-and-data-access/_static/image2.png "Adding an Existing Item")</span></span>

    <span data-ttu-id="be9d3-164">*Dodawanie istniejącego elementu*</span><span class="sxs-lookup"><span data-stu-id="be9d3-164">*Adding an Existing Item*</span></span>

    <span data-ttu-id="be9d3-165">![Plik bazy danych MvcMusicStore. mdf](aspnet-mvc-4-models-and-data-access/_static/image3.png "Plik bazy danych MvcMusicStore. mdf")</span><span class="sxs-lookup"><span data-stu-id="be9d3-165">![MvcMusicStore.mdf database file](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf database file")</span></span>

    <span data-ttu-id="be9d3-166">*Plik bazy danych MvcMusicStore. mdf*</span><span class="sxs-lookup"><span data-stu-id="be9d3-166">*MvcMusicStore.mdf database file*</span></span>

    <span data-ttu-id="be9d3-167">Baza danych została dodana do projektu.</span><span class="sxs-lookup"><span data-stu-id="be9d3-167">The database has been added to the project.</span></span> <span data-ttu-id="be9d3-168">Nawet jeśli baza danych znajduje się w rozwiązaniu, można wykonać zapytanie i zaktualizować ją, ponieważ była hostowana na innym serwerze bazy danych.</span><span class="sxs-lookup"><span data-stu-id="be9d3-168">Even when the database is located inside the solution, you can query and update it as it was hosted in a different database server.</span></span>

    <span data-ttu-id="be9d3-169">![Baza danych MvcMusicStore w Eksplorator rozwiązań](aspnet-mvc-4-models-and-data-access/_static/image4.png "Baza danych MvcMusicStore w Eksplorator rozwiązań")</span><span class="sxs-lookup"><span data-stu-id="be9d3-169">![MvcMusicStore database in Solution Explorer](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore database in Solution Explorer")</span></span>

    <span data-ttu-id="be9d3-170">*Baza danych MvcMusicStore w Eksplorator rozwiązań*</span><span class="sxs-lookup"><span data-stu-id="be9d3-170">*MvcMusicStore database in Solution Explorer*</span></span>
3. <span data-ttu-id="be9d3-171">Sprawdź połączenie z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="be9d3-171">Verify the connection to the database.</span></span> <span data-ttu-id="be9d3-172">Aby to zrobić, kliknij dwukrotnie plik **MvcMusicStore. mdf** , aby nawiązać połączenie.</span><span class="sxs-lookup"><span data-stu-id="be9d3-172">To do this, double-click **MvcMusicStore.mdf** to establish a connection.</span></span>

    <span data-ttu-id="be9d3-173">![Łączenie z MvcMusicStore. mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "Łączenie z MvcMusicStore. mdf")</span><span class="sxs-lookup"><span data-stu-id="be9d3-173">![Connecting to MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "Connecting to MvcMusicStore.mdf")</span></span>

    <span data-ttu-id="be9d3-174">*Łączenie z MvcMusicStore. mdf*</span><span class="sxs-lookup"><span data-stu-id="be9d3-174">*Connecting to MvcMusicStore.mdf*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a><span data-ttu-id="be9d3-175">Zadanie 2 — Tworzenie modelu danych</span><span class="sxs-lookup"><span data-stu-id="be9d3-175">Task 2 - Creating a Data Model</span></span>

<span data-ttu-id="be9d3-176">W tym zadaniu utworzysz model danych, który będzie mógł korzystać z bazy danych dodanej w poprzednim zadaniu.</span><span class="sxs-lookup"><span data-stu-id="be9d3-176">In this task, you will create a data model to interact with the database added in the previous task.</span></span>

1. <span data-ttu-id="be9d3-177">Utwórz model danych, który będzie reprezentować bazę danych.</span><span class="sxs-lookup"><span data-stu-id="be9d3-177">Create a data model that will represent the database.</span></span> <span data-ttu-id="be9d3-178">W tym celu w Eksplorator rozwiązań kliknij prawym przyciskiem myszy folder **modele** , wskaż polecenie **Dodaj** , a następnie kliknij pozycję **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="be9d3-178">To do this, in Solution Explorer right-click the **Models** folder, point to **Add** and then click **New Item**.</span></span> <span data-ttu-id="be9d3-179">W oknie dialogowym **Dodaj nowy element** wybierz szablon **danych** , a następnie element **ADO.NET Entity Data Model** .</span><span class="sxs-lookup"><span data-stu-id="be9d3-179">In the **Add New Item** dialog, select the **Data** template and then the **ADO.NET Entity Data Model** item.</span></span> <span data-ttu-id="be9d3-180">Zmień nazwę modelu danych na **StoreDB. edmx** , a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="be9d3-180">Change the data model name to **StoreDB.edmx** and click **Add**.</span></span>

    <span data-ttu-id="be9d3-181">![Dodawanie Entity Data Model ADO.NET StoreDB](aspnet-mvc-4-models-and-data-access/_static/image6.png "Dodawanie Entity Data Model ADO.NET StoreDB")</span><span class="sxs-lookup"><span data-stu-id="be9d3-181">![Adding the StoreDB ADO.NET Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "Adding the StoreDB ADO.NET Entity Data Model")</span></span>

    <span data-ttu-id="be9d3-182">*Dodawanie Entity Data Model ADO.NET StoreDB*</span><span class="sxs-lookup"><span data-stu-id="be9d3-182">*Adding the StoreDB ADO.NET Entity Data Model*</span></span>
2. <span data-ttu-id="be9d3-183">Zostanie wyświetlony **kreator Entity Data Model** .</span><span class="sxs-lookup"><span data-stu-id="be9d3-183">The **Entity Data Model Wizard** will appear.</span></span> <span data-ttu-id="be9d3-184">Ten Kreator przeprowadzi Cię przez proces tworzenia warstwy modelu.</span><span class="sxs-lookup"><span data-stu-id="be9d3-184">This wizard will guide you through the creation of the model layer.</span></span> <span data-ttu-id="be9d3-185">Ponieważ model powinien być tworzony na podstawie ostatnio dodanej bazy danych, wybierz pozycję **Generuj z bazy danych** i kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="be9d3-185">Since the model should be created based on the existing database recently added, select **Generate from database** and click **Next**.</span></span>

    <span data-ttu-id="be9d3-186">![Wybieranie zawartości modelu](aspnet-mvc-4-models-and-data-access/_static/image7.png "Wybieranie zawartości modelu")</span><span class="sxs-lookup"><span data-stu-id="be9d3-186">![Choosing the model content](aspnet-mvc-4-models-and-data-access/_static/image7.png "Choosing the model content")</span></span>

    <span data-ttu-id="be9d3-187">*Wybieranie zawartości modelu*</span><span class="sxs-lookup"><span data-stu-id="be9d3-187">*Choosing the model content*</span></span>
3. <span data-ttu-id="be9d3-188">Ponieważ generujesz model z bazy danych, musisz określić połączenie, które ma być używane.</span><span class="sxs-lookup"><span data-stu-id="be9d3-188">Since you are generating a model from a database, you will need to specify the connection to use.</span></span> <span data-ttu-id="be9d3-189">Kliknij pozycję **nowe połączenie**.</span><span class="sxs-lookup"><span data-stu-id="be9d3-189">Click **New Connection**.</span></span>
4. <span data-ttu-id="be9d3-190">Wybierz **plik bazy danych Microsoft SQL Server** i kliknij przycisk **Kontynuuj**.</span><span class="sxs-lookup"><span data-stu-id="be9d3-190">Select **Microsoft SQL Server Database File** and click **Continue**.</span></span>

    <span data-ttu-id="be9d3-191">![Wybieranie źródła danych](aspnet-mvc-4-models-and-data-access/_static/image8.png "Wybieranie źródła danych")</span><span class="sxs-lookup"><span data-stu-id="be9d3-191">![Choose data source](aspnet-mvc-4-models-and-data-access/_static/image8.png "Choose data source")</span></span>

    <span data-ttu-id="be9d3-192">*Okno dialogowe Wybieranie źródła danych*</span><span class="sxs-lookup"><span data-stu-id="be9d3-192">*Choose data source dialog*</span></span>
5. <span data-ttu-id="be9d3-193">Kliknij przycisk **Przeglądaj** i wybierz bazę danych **MvcMusicStore. mdf** , która znajduje się w folderze **danych\_aplikacji** , a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="be9d3-193">Click **Browse** and select the database **MvcMusicStore.mdf** you located in the **App\_Data** folder and click **OK**.</span></span>

    <span data-ttu-id="be9d3-194">![Właściwości połączenia](aspnet-mvc-4-models-and-data-access/_static/image9.png "Connection properties (Właściwości połączenia)")</span><span class="sxs-lookup"><span data-stu-id="be9d3-194">![Connection properties](aspnet-mvc-4-models-and-data-access/_static/image9.png "Connection properties")</span></span>

    <span data-ttu-id="be9d3-195">*Właściwości połączenia*</span><span class="sxs-lookup"><span data-stu-id="be9d3-195">*Connection properties*</span></span>
6. <span data-ttu-id="be9d3-196">Wygenerowana Klasa powinna mieć taką samą nazwę jak parametry połączenia jednostki, więc Zmień jej nazwę na **MusicStoreEntities** i kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="be9d3-196">The generated class should have the same name as the entity connection string, so change its name to **MusicStoreEntities** and click **Next**.</span></span>

    <span data-ttu-id="be9d3-197">![Wybieranie połączenia danych](aspnet-mvc-4-models-and-data-access/_static/image10.png "Wybieranie połączenia danych")</span><span class="sxs-lookup"><span data-stu-id="be9d3-197">![Choosing the data connection](aspnet-mvc-4-models-and-data-access/_static/image10.png "Choosing the data connection")</span></span>

    <span data-ttu-id="be9d3-198">*Wybieranie połączenia danych*</span><span class="sxs-lookup"><span data-stu-id="be9d3-198">*Choosing the data connection*</span></span>
7. <span data-ttu-id="be9d3-199">Wybierz obiekty bazy danych, które mają być używane.</span><span class="sxs-lookup"><span data-stu-id="be9d3-199">Choose the database objects to use.</span></span> <span data-ttu-id="be9d3-200">Ponieważ model jednostki będzie używać tylko tabel bazy danych, wybierz opcję **tabele** i upewnij się, że są także zaznaczone opcje **Uwzględnij kolumny klucza obcego w modelu** i **pluralize lub nazwom** .</span><span class="sxs-lookup"><span data-stu-id="be9d3-200">As the Entity Model will use just the database's tables, select the **Tables** option, and make sure that the **Include foreign key columns in the model** and **Pluralize or singularize generated object names** options are also selected.</span></span> <span data-ttu-id="be9d3-201">Zmień przestrzeń nazw modelu na **MvcMusicStore. model** i kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="be9d3-201">Change the Model Namespace to **MvcMusicStore.Model** and click **Finish**.</span></span>

    <span data-ttu-id="be9d3-202">![Wybieranie obiektów bazy danych](aspnet-mvc-4-models-and-data-access/_static/image11.png "Wybieranie obiektów bazy danych")</span><span class="sxs-lookup"><span data-stu-id="be9d3-202">![Choosing the database objects](aspnet-mvc-4-models-and-data-access/_static/image11.png "Choosing the database objects")</span></span>

    <span data-ttu-id="be9d3-203">*Wybieranie obiektów bazy danych*</span><span class="sxs-lookup"><span data-stu-id="be9d3-203">*Choosing the database objects*</span></span>

    > [!NOTE]
    > <span data-ttu-id="be9d3-204">Jeśli zostanie wyświetlone okno dialogowe ostrzeżenia o zabezpieczeniach, kliknij przycisk **OK** , aby uruchomić szablon i wygenerować klasy dla jednostek modelu.</span><span class="sxs-lookup"><span data-stu-id="be9d3-204">If a Security Warning dialog is shown, click **OK** to run the template and generate the classes for the model entities.</span></span>
8. <span data-ttu-id="be9d3-205">Zostanie wyświetlony diagram jednostki dla bazy danych, podczas gdy zostanie utworzona oddzielna Klasa, która mapuje każdą tabelę na bazę danych.</span><span class="sxs-lookup"><span data-stu-id="be9d3-205">An entity diagram for the database will appear, while a separate class that maps each table to the database will be created.</span></span> <span data-ttu-id="be9d3-206">Na przykład tabela **albumy** będzie reprezentowana przez klasę **albumu** , gdzie każda kolumna w tabeli zostanie zamapowana na właściwość klasy.</span><span class="sxs-lookup"><span data-stu-id="be9d3-206">For example, the **Albums** table will be represented by an **Album** class, where each column in the table will map to a class property.</span></span> <span data-ttu-id="be9d3-207">Pozwoli to na wykonywanie zapytań i pracy z obiektami, które reprezentują wiersze w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="be9d3-207">This will allow you to query and work with objects that represent rows in the database.</span></span>

    <span data-ttu-id="be9d3-208">![Diagram jednostek](aspnet-mvc-4-models-and-data-access/_static/image12.png "Diagram jednostek")</span><span class="sxs-lookup"><span data-stu-id="be9d3-208">![Entity diagram](aspnet-mvc-4-models-and-data-access/_static/image12.png "Entity diagram")</span></span>

    <span data-ttu-id="be9d3-209">*Diagram jednostek*</span><span class="sxs-lookup"><span data-stu-id="be9d3-209">*Entity diagram*</span></span>

    > [!NOTE]
    > <span data-ttu-id="be9d3-210">Szablony T4 (. tt) uruchamiają kod w celu wygenerowania klas jednostek i zastąpią istniejące klasy o tej samej nazwie.</span><span class="sxs-lookup"><span data-stu-id="be9d3-210">The T4 templates (.tt) run code to generate the entities classes and will overwrite the existing classes with the same name.</span></span> <span data-ttu-id="be9d3-211">W tym przykładzie klasy &quot;albumu&quot;, &quot;gatunek&quot; i &quot;artyści&quot; zostały zastąpione wygenerowanym kodem.</span><span class="sxs-lookup"><span data-stu-id="be9d3-211">In this example, the classes &quot;Album&quot;, &quot;Genre&quot; and &quot;Artist&quot; were overwritten with the generated code.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a><span data-ttu-id="be9d3-212">Zadanie 3 — Kompilowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="be9d3-212">Task 3 - Building the Application</span></span>

<span data-ttu-id="be9d3-213">W tym zadaniu sprawdzisz, że mimo że generacja modelu usunęła klasy **album**, **gatunek** i model **wykonawcy** , projekt zostanie pomyślnie skompilowany przy użyciu nowych klas modelu danych.</span><span class="sxs-lookup"><span data-stu-id="be9d3-213">In this task, you will check that, although the model generation have removed the **Album**, **Genre** and **Artist** model classes, the project builds successfully by using the new data model classes.</span></span>

1. <span data-ttu-id="be9d3-214">Skompiluj projekt, wybierając element menu **kompilacja** , a następnie **Skompiluj MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="be9d3-214">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="be9d3-215">![Kompilowanie projektu](aspnet-mvc-4-models-and-data-access/_static/image13.png "Kompilowanie projektu")</span><span class="sxs-lookup"><span data-stu-id="be9d3-215">![Building the project](aspnet-mvc-4-models-and-data-access/_static/image13.png "Building the project")</span></span>

    <span data-ttu-id="be9d3-216">*Kompilowanie projektu*</span><span class="sxs-lookup"><span data-stu-id="be9d3-216">*Building the project*</span></span>
2. <span data-ttu-id="be9d3-217">Projekt zostanie pomyślnie skompilowany.</span><span class="sxs-lookup"><span data-stu-id="be9d3-217">The project builds successfully.</span></span> <span data-ttu-id="be9d3-218">Dlaczego nadal działa?</span><span class="sxs-lookup"><span data-stu-id="be9d3-218">Why does it still work?</span></span> <span data-ttu-id="be9d3-219">Działa, ponieważ tabele bazy danych zawierają pola zawierające właściwości, które były używane w **albumie** i **gatunku**usuniętych klas.</span><span class="sxs-lookup"><span data-stu-id="be9d3-219">It works because the database tables have fields that include the properties that you were using in the removed classes **Album** and **Genre**.</span></span>

    <span data-ttu-id="be9d3-220">![Kompilacja powiodła się](aspnet-mvc-4-models-and-data-access/_static/image14.png "Kompilacja powiodła się")</span><span class="sxs-lookup"><span data-stu-id="be9d3-220">![Builds succeeded](aspnet-mvc-4-models-and-data-access/_static/image14.png "Builds succeeded")</span></span>

    <span data-ttu-id="be9d3-221">*Kompilacja powiodła się*</span><span class="sxs-lookup"><span data-stu-id="be9d3-221">*Builds succeeded*</span></span>
3. <span data-ttu-id="be9d3-222">Gdy Projektant wyświetla jednostki w formacie diagramu, są one w rzeczywistości C# klasą.</span><span class="sxs-lookup"><span data-stu-id="be9d3-222">While the designer displays the entities in a diagram format, they are really C# classes.</span></span> <span data-ttu-id="be9d3-223">Rozwiń węzeł **StoreDB. edmx** w Eksplorator rozwiązań a następnie pozycję **StoreDB.tt**, zobaczysz nowe wygenerowane jednostki.</span><span class="sxs-lookup"><span data-stu-id="be9d3-223">Expand the **StoreDB.edmx** node in the Solution Explorer and then **StoreDB.tt**, you will see the new generated entities.</span></span>

    <span data-ttu-id="be9d3-224">![Wygenerowane pliki](aspnet-mvc-4-models-and-data-access/_static/image15.png "Wygenerowane pliki")</span><span class="sxs-lookup"><span data-stu-id="be9d3-224">![Generated files](aspnet-mvc-4-models-and-data-access/_static/image15.png "Generated files")</span></span>

    <span data-ttu-id="be9d3-225">*Wygenerowane pliki*</span><span class="sxs-lookup"><span data-stu-id="be9d3-225">*Generated files*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="be9d3-226">Zadanie 4 — wykonywanie zapytania dotyczącego bazy danych</span><span class="sxs-lookup"><span data-stu-id="be9d3-226">Task 4 - Querying the Database</span></span>

<span data-ttu-id="be9d3-227">W tym zadaniu zostanie zaktualizowana Klasa StoreController, tak aby zamiast korzystania z danych stałe przeprowadzili zapytanie do bazy danych w celu pobrania informacji.</span><span class="sxs-lookup"><span data-stu-id="be9d3-227">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will query the database to retrieve the information.</span></span>

1. <span data-ttu-id="be9d3-228">Otwórz **Controllers\StoreController.cs** i Dodaj następujące pole do klasy, aby pomieścić wystąpienie klasy **MusicStoreEntities** o nazwie **storeDB**:</span><span class="sxs-lookup"><span data-stu-id="be9d3-228">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="be9d3-229">(Fragment kodu — *modele i dostęp do danych — Ex1 storeDB*)</span><span class="sxs-lookup"><span data-stu-id="be9d3-229">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
2. <span data-ttu-id="be9d3-230">Klasa **MusicStoreEntities** uwidacznia Właściwość kolekcji dla każdej tabeli w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="be9d3-230">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="be9d3-231">Aktualizowanie metody akcji **przeglądania** w celu pobrania gatunku z wszystkimi **albumami**.</span><span class="sxs-lookup"><span data-stu-id="be9d3-231">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="be9d3-232">(Fragment kodu — *modele i dostęp do danych — Ex1 Store Browse*)</span><span class="sxs-lookup"><span data-stu-id="be9d3-232">(Code Snippet - *Models And Data Access - Ex1 Store Browse*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

    > [!NOTE]
    > <span data-ttu-id="be9d3-233">Używasz możliwości platformy .NET o nazwie **LINQ** (załączonej do języka) do pisania wyrażeń zapytania o jednoznacznie określonym typie względem tych kolekcji, które będą wykonywać kod względem bazy danych i zwracać obiekty, dla których można programować.</span><span class="sxs-lookup"><span data-stu-id="be9d3-233">You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.</span></span>
    > 
    > <span data-ttu-id="be9d3-234">Więcej informacji na temat programu LINQ można znaleźć w [witrynie MSDN](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="be9d3-234">For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).</span></span>
3. <span data-ttu-id="be9d3-235">Zaktualizuj metodę akcji **indeksu** w celu pobrania wszystkich gatunków.</span><span class="sxs-lookup"><span data-stu-id="be9d3-235">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="be9d3-236">(Fragment kodu — *modele i dostęp do danych — indeks magazynu Ex1*)</span><span class="sxs-lookup"><span data-stu-id="be9d3-236">(Code Snippet - *Models And Data Access - Ex1 Store Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
4. <span data-ttu-id="be9d3-237">Zaktualizuj metodę akcji **indeksu** w celu pobrania wszystkich gatunków i przekształcenia kolekcji na listę.</span><span class="sxs-lookup"><span data-stu-id="be9d3-237">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="be9d3-238">(Fragment kodu — *modele i dostęp do danych — Ex1 Store GenreMenu*)</span><span class="sxs-lookup"><span data-stu-id="be9d3-238">(Code Snippet - *Models And Data Access - Ex1 Store GenreMenu*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="be9d3-239">Zadanie 5 — Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="be9d3-239">Task 5 - Running the Application</span></span>

<span data-ttu-id="be9d3-240">W tym zadaniu sprawdzisz, że na stronie indeks sklepu będą teraz wyświetlane gatunki przechowywane w bazie danych zamiast stałe.</span><span class="sxs-lookup"><span data-stu-id="be9d3-240">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="be9d3-241">Nie ma potrzeby zmiany szablonu widoku, ponieważ **StoreController** zwraca te same jednostki co poprzednio, chociaż ten czas będzie pochodzący z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="be9d3-241">There is no need to change the View template because the **StoreController** is returning the same entities as before, although this time the data will come from the database.</span></span>

1. <span data-ttu-id="be9d3-242">Skompiluj ponownie rozwiązanie i naciśnij klawisz **F5** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="be9d3-242">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="be9d3-243">Projekt zostanie uruchomiony na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="be9d3-243">The project starts in the Home page.</span></span> <span data-ttu-id="be9d3-244">Sprawdź, czy menu **gatunek** nie jest już listą stałe, a dane są bezpośrednio pobierane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="be9d3-244">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    <span data-ttu-id="be9d3-246">*Przeglądanie gatunków z bazy danych*</span><span class="sxs-lookup"><span data-stu-id="be9d3-246">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="be9d3-247">Teraz przejdź do dowolnego gatunku i sprawdź, czy albumy są wypełnione z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="be9d3-247">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="be9d3-248">![Przeglądanie albumów z bazy danych](aspnet-mvc-4-models-and-data-access/_static/image17.png "Przeglądanie albumów z bazy danych")</span><span class="sxs-lookup"><span data-stu-id="be9d3-248">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image17.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="be9d3-249">*Przeglądanie albumów z bazy danych*</span><span class="sxs-lookup"><span data-stu-id="be9d3-249">*Browsing Albums from the database*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a><span data-ttu-id="be9d3-250">Ćwiczenie 2: Tworzenie bazy danych przy użyciu Code First</span><span class="sxs-lookup"><span data-stu-id="be9d3-250">Exercise 2: Creating a Database Using Code First</span></span>

<span data-ttu-id="be9d3-251">W tym ćwiczeniu dowiesz się, jak za pomocą podejścia Code First utworzyć bazę danych z tabelami aplikacji MusicStore oraz jak uzyskać dostęp do swoich danych.</span><span class="sxs-lookup"><span data-stu-id="be9d3-251">In this exercise, you will learn how to use the Code First approach to create a database with the tables of the MusicStore application, and how to access its data.</span></span>

<span data-ttu-id="be9d3-252">Po wygenerowaniu modelu zmodyfikujesz StoreController, aby udostępnić szablon widoku z danymi pobranymi z bazy danych, zamiast używać wartości stałe.</span><span class="sxs-lookup"><span data-stu-id="be9d3-252">Once the model is generated, you will modify the StoreController to provide the View template with the data taken from the database, instead of using hardcoded values.</span></span>

> [!NOTE]
> <span data-ttu-id="be9d3-253">Jeśli wykonano 1 ćwiczenie i już pracowałeś z podejściem Database First, dowiesz się, jak uzyskać te same wyniki przy użyciu innego procesu.</span><span class="sxs-lookup"><span data-stu-id="be9d3-253">If you have completed Exercise 1 and have already worked with the Database First approach, you will now learn how to get the same results with a different process.</span></span> <span data-ttu-id="be9d3-254">Zadania, które są wspólne w ćwiczeniu 1, zostały oznaczone, aby ułatwić odczytywanie.</span><span class="sxs-lookup"><span data-stu-id="be9d3-254">The tasks that are in common with Exercise 1 have been marked to make your reading easier.</span></span> <span data-ttu-id="be9d3-255">Jeśli nie zakończysz ćwiczenia 1, ale chcesz poznać podejście Code First, możesz zacząć od tego ćwiczenia i uzyskać pełen zakres tematu.</span><span class="sxs-lookup"><span data-stu-id="be9d3-255">If you have not completed Exercise 1 but would like to learn the Code First approach, you can start from this exercise and get a full coverage of the topic.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a><span data-ttu-id="be9d3-256">Zadanie 1 — zapełnianie przykładowych danych</span><span class="sxs-lookup"><span data-stu-id="be9d3-256">Task 1 - Populating Sample Data</span></span>

<span data-ttu-id="be9d3-257">W tym zadaniu zostanie wypełniona baza danych z przykładowymi danymi, gdy zostanie ona początkowo utworzona przy użyciu kodu.</span><span class="sxs-lookup"><span data-stu-id="be9d3-257">In this task, you will populate the database with sample data when it is initially created using Code-First.</span></span>

1. <span data-ttu-id="be9d3-258">Otwórz rozwiązanie **BEGIN** znajdujące się w lokalizacji **Source/Ex2-CreatingADatabaseCodeFirst/BEGIN/** folder.</span><span class="sxs-lookup"><span data-stu-id="be9d3-258">Open the **Begin** solution located at **Source/Ex2-CreatingADatabaseCodeFirst/Begin/** folder.</span></span> <span data-ttu-id="be9d3-259">W przeciwnym razie można nadal korzystać z rozwiązania **końcowego** uzyskanego przez zakończenie poprzedniego ćwiczenia.</span><span class="sxs-lookup"><span data-stu-id="be9d3-259">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="be9d3-260">Jeśli otwarto **udostępnione rozwiązanie** , należy pobrać brakujące pakiety NuGet przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="be9d3-260">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="be9d3-261">W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="be9d3-261">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="be9d3-262">W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.</span><span class="sxs-lookup"><span data-stu-id="be9d3-262">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="be9d3-263">Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="be9d3-263">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="be9d3-264">Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="be9d3-264">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="be9d3-265">Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu.</span><span class="sxs-lookup"><span data-stu-id="be9d3-265">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="be9d3-266">To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="be9d3-266">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="be9d3-267">Dodaj plik **SampleData.cs** do folderu **models** .</span><span class="sxs-lookup"><span data-stu-id="be9d3-267">Add the **SampleData.cs** file to the **Models** folder.</span></span> <span data-ttu-id="be9d3-268">W tym celu kliknij prawym przyciskiem myszy folder **modele** , wskaż polecenie **Dodaj** , a następnie kliknij pozycję **istniejący element**.</span><span class="sxs-lookup"><span data-stu-id="be9d3-268">To do that, right-click **Models** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="be9d3-269">Przejdź do **\Source\Assets** i wybierz plik **SampleData.cs** .</span><span class="sxs-lookup"><span data-stu-id="be9d3-269">Browse to **\Source\Assets** and select the **SampleData.cs** file.</span></span>

    <span data-ttu-id="be9d3-270">![Przykładowy kod uzupełniający dane](aspnet-mvc-4-models-and-data-access/_static/image18.png "Przykładowy kod uzupełniający dane")</span><span class="sxs-lookup"><span data-stu-id="be9d3-270">![Sample data populate code](aspnet-mvc-4-models-and-data-access/_static/image18.png "Sample data populate code")</span></span>

    <span data-ttu-id="be9d3-271">*Przykładowy kod uzupełniający dane*</span><span class="sxs-lookup"><span data-stu-id="be9d3-271">*Sample data populate code*</span></span>
3. <span data-ttu-id="be9d3-272">Otwórz plik **Global.asax.cs** i Dodaj następujące instrukcje *using* .</span><span class="sxs-lookup"><span data-stu-id="be9d3-272">Open the **Global.asax.cs** file and add the following *using* statements.</span></span>

    <span data-ttu-id="be9d3-273">(Fragment kodu — *modele i dostęp do danych — Ex2 Global asax using*)</span><span class="sxs-lookup"><span data-stu-id="be9d3-273">(Code Snippet - *Models And Data Access - Ex2 Global Asax Usings*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
4. <span data-ttu-id="be9d3-274">Aby ustawić inicjatora bazy danych, w metodzie **Start\_aplikacji** Dodaj następujący wiersz.</span><span class="sxs-lookup"><span data-stu-id="be9d3-274">In the **Application\_Start()** method add the following line to set the database initializer.</span></span>

    <span data-ttu-id="be9d3-275">(Fragment kodu — *modele i dostęp do danych — Ex2 Global asax Setinitializeer*)</span><span class="sxs-lookup"><span data-stu-id="be9d3-275">(Code Snippet - *Models And Data Access - Ex2 Global Asax SetInitializer*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a><span data-ttu-id="be9d3-276">Zadanie 2 — Konfigurowanie połączenia z bazą danych</span><span class="sxs-lookup"><span data-stu-id="be9d3-276">Task 2 - Configuring the connection to the Database</span></span>

<span data-ttu-id="be9d3-277">Teraz, gdy baza danych została już dodana do projektu, należy napisać w pliku **Web. config** parametry połączenia.</span><span class="sxs-lookup"><span data-stu-id="be9d3-277">Now that you have already added a database to our project, you will write in the **Web.config** file the connection string.</span></span>

1. <span data-ttu-id="be9d3-278">Dodaj parametry połączenia w **pliku Web. config**. W tym celu Otwórz **plik Web. config** w katalogu głównym projektu i Zastąp ciąg połączenia o nazwie DefaultConnection tym wierszem w sekcji **&lt;connectionStrings&gt;** :</span><span class="sxs-lookup"><span data-stu-id="be9d3-278">Add a connection string at **Web.config**. To do that, open **Web.config** at project root and replace the connection string named DefaultConnection with this line in the **&lt;connectionStrings&gt;** section:</span></span>

    <span data-ttu-id="be9d3-279">![Lokalizacja pliku Web. config](aspnet-mvc-4-models-and-data-access/_static/image19.png "Lokalizacja pliku Web. config")</span><span class="sxs-lookup"><span data-stu-id="be9d3-279">![Web.config file location](aspnet-mvc-4-models-and-data-access/_static/image19.png "Web.config file location")</span></span>

    <span data-ttu-id="be9d3-280">*Lokalizacja pliku Web. config*</span><span class="sxs-lookup"><span data-stu-id="be9d3-280">*Web.config file location*</span></span>

    [!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a><span data-ttu-id="be9d3-281">Zadanie 3 — Praca z modelem</span><span class="sxs-lookup"><span data-stu-id="be9d3-281">Task 3 - Working with the Model</span></span>

<span data-ttu-id="be9d3-282">Teraz, gdy połączenie z bazą danych zostało już skonfigurowane, możesz połączyć model z tabelami baz danych.</span><span class="sxs-lookup"><span data-stu-id="be9d3-282">Now that you have already configured the connection to the database, you will link the model with the database tables.</span></span> <span data-ttu-id="be9d3-283">W tym zadaniu utworzysz klasę, która będzie połączona z bazą danych Code First.</span><span class="sxs-lookup"><span data-stu-id="be9d3-283">In this task, you will create a class that will be linked to the database with Code First.</span></span> <span data-ttu-id="be9d3-284">Należy pamiętać, że istnieje Klasa modelu POCO, która powinna zostać zmodyfikowana.</span><span class="sxs-lookup"><span data-stu-id="be9d3-284">Remember that there is an existent POCO model class that should be modified.</span></span>

> [!NOTE]
> <span data-ttu-id="be9d3-285">Jeśli wykonasz ćwiczenia 1, ten krok został wykonany przez kreatora.</span><span class="sxs-lookup"><span data-stu-id="be9d3-285">If you completed Exercise 1, you will note that this step was performed by a wizard.</span></span> <span data-ttu-id="be9d3-286">Wykonując Code First, ręcznie utworzysz klasy, które zostaną połączone z jednostkami danych.</span><span class="sxs-lookup"><span data-stu-id="be9d3-286">By doing Code First, you will manually create classes that will be linked to data entities.</span></span>

1. <span data-ttu-id="be9d3-287">Otwórz **gatunek** klasy modelu poco z folderu **models** Project i Dołącz identyfikator.</span><span class="sxs-lookup"><span data-stu-id="be9d3-287">Open the POCO model class **Genre** from **Models** project folder and include an ID.</span></span> <span data-ttu-id="be9d3-288">Użyj właściwości int o nazwie **GenreId**.</span><span class="sxs-lookup"><span data-stu-id="be9d3-288">Use an int property with the name **GenreId**.</span></span>

    <span data-ttu-id="be9d3-289">(Fragment kodu — *modele i dostęp do danych — Ex2 Code First gatunek*)</span><span class="sxs-lookup"><span data-stu-id="be9d3-289">(Code Snippet - *Models And Data Access - Ex2 Code First Genre*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

    > [!NOTE]
    > <span data-ttu-id="be9d3-290">Aby można było korzystać z Konwencji Code First, gatunek klasy musi mieć właściwość klucza podstawowego, która zostanie automatycznie wykryta.</span><span class="sxs-lookup"><span data-stu-id="be9d3-290">To work with Code First conventions, the class Genre must have a primary key property that will be automatically detected.</span></span>
    > 
    > <span data-ttu-id="be9d3-291">Więcej informacji na temat Konwencji Code First można znaleźć w tym [artykule w witrynie MSDN](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="be9d3-291">You can read more about Code First Conventions in this [msdn article](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).</span></span>
2. <span data-ttu-id="be9d3-292">Teraz Otwórz **album** klasy modelu poco z folderu **models** Project i Dołącz klucze obce, Utwórz właściwości o nazwach **GenreId** i **ArtistId**.</span><span class="sxs-lookup"><span data-stu-id="be9d3-292">Now, open the POCO model class **Album** from **Models** project folder and include the foreign keys, create properties with the names **GenreId** and **ArtistId**.</span></span> <span data-ttu-id="be9d3-293">Ta klasa ma już **GenreId** klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="be9d3-293">This class already have the **GenreId** for the primary key.</span></span>

    <span data-ttu-id="be9d3-294">(Fragment kodu — *modele i dostęp do danych — Ex2 Code First albumu*)</span><span class="sxs-lookup"><span data-stu-id="be9d3-294">(Code Snippet - *Models And Data Access - Ex2 Code First Album*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
3. <span data-ttu-id="be9d3-295">Otwórz **wykonawcę** klasy modelu POCO i Uwzględnij Właściwość **ArtistId** .</span><span class="sxs-lookup"><span data-stu-id="be9d3-295">Open the POCO model class **Artist** and include the **ArtistId** property.</span></span>

    <span data-ttu-id="be9d3-296">(Fragment kodu — *modele i dostęp do danych — Ex2 Code First wykonawcze*)</span><span class="sxs-lookup"><span data-stu-id="be9d3-296">(Code Snippet - *Models And Data Access - Ex2 Code First Artist*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
4. <span data-ttu-id="be9d3-297">Kliknij prawym przyciskiem myszy folder **modele** projektu i wybierz polecenie **Dodaj | Klasa**.</span><span class="sxs-lookup"><span data-stu-id="be9d3-297">Right-click the **Models** project folder and select **Add | Class**.</span></span> <span data-ttu-id="be9d3-298">Nazwij plik **MusicStoreEntities.cs**.</span><span class="sxs-lookup"><span data-stu-id="be9d3-298">Name the file **MusicStoreEntities.cs**.</span></span> <span data-ttu-id="be9d3-299">Następnie kliknij przycisk **Dodaj.**</span><span class="sxs-lookup"><span data-stu-id="be9d3-299">Then, click **Add.**</span></span>

    <span data-ttu-id="be9d3-300">![Dodawanie klasy](aspnet-mvc-4-models-and-data-access/_static/image20.png "Dodawanie klasy")</span><span class="sxs-lookup"><span data-stu-id="be9d3-300">![Adding a class](aspnet-mvc-4-models-and-data-access/_static/image20.png "Adding a class")</span></span>

    <span data-ttu-id="be9d3-301">*Dodawanie nowego elementu*</span><span class="sxs-lookup"><span data-stu-id="be9d3-301">*Adding a new item*</span></span>

    <span data-ttu-id="be9d3-302">![Dodawanie elementu 'klasa](aspnet-mvc-4-models-and-data-access/_static/image21.png "Dodawanie elementu 'klasa")</span><span class="sxs-lookup"><span data-stu-id="be9d3-302">![Adding a class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "Adding a class2")</span></span>

    <span data-ttu-id="be9d3-303">*Dodawanie klasy*</span><span class="sxs-lookup"><span data-stu-id="be9d3-303">*Adding a class*</span></span>
5. <span data-ttu-id="be9d3-304">Otwórz właśnie utworzoną klasę, **MusicStoreEntities.cs**i Uwzględnij przestrzeń nazw **System. Data. Entity** i **System. Data. Entity. Infrastructure**.</span><span class="sxs-lookup"><span data-stu-id="be9d3-304">Open the class you have just created, **MusicStoreEntities.cs**, and include the namespaces **System.Data.Entity** and **System.Data.Entity.Infrastructure**.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
6. <span data-ttu-id="be9d3-305">Zastąp deklarację klasy, aby zwiększyć klasę **DbContext** : Zadeklaruj publiczną **nieogólnymi** i Zastąp metodę **OnModelCreating** .</span><span class="sxs-lookup"><span data-stu-id="be9d3-305">Replace the class declaration to extend the **DbContext** class: declare a public **DBSet** and override **OnModelCreating** method.</span></span> <span data-ttu-id="be9d3-306">Po wykonaniu tego kroku otrzymasz klasę domeny, która będzie łączyć model z Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="be9d3-306">After this step you will get a domain class that will link your model with the Entity Framework.</span></span> <span data-ttu-id="be9d3-307">Aby to zrobić, Zastąp kod klasy następującym:</span><span class="sxs-lookup"><span data-stu-id="be9d3-307">In order to do that, replace the class code with the following:</span></span>

    <span data-ttu-id="be9d3-308">(Fragment kodu — *modele i dostęp do danych — Ex2 Code First MusicStoreEntities*)</span><span class="sxs-lookup"><span data-stu-id="be9d3-308">(Code Snippet - *Models And Data Access - Ex2 Code First MusicStoreEntities*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

> [!NOTE]
> <span data-ttu-id="be9d3-309">Dzięki Entity Framework **DbContext** i **nieogólnymi** będzie można badać gatunek klasy poco.</span><span class="sxs-lookup"><span data-stu-id="be9d3-309">With Entity Framework **DbContext** and **DBSet** you will be able to query the POCO class Genre.</span></span> <span data-ttu-id="be9d3-310">Rozszerzając metodę **OnModelCreating** , określasz w **kodzie** , jak gatunek zostanie zamapowany do tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="be9d3-310">By extending **OnModelCreating** method, you are specifying in the **code** how Genre will be mapped to a database table.</span></span> <span data-ttu-id="be9d3-311">Więcej informacji na temat DbContext i Nieogólnymi można znaleźć w tym artykule MSDN: [link](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)</span><span class="sxs-lookup"><span data-stu-id="be9d3-311">You can find more information about DBContext and DBSet in this msdn article: [link](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="be9d3-312">Zadanie 4 — wykonywanie zapytania dotyczącego bazy danych</span><span class="sxs-lookup"><span data-stu-id="be9d3-312">Task 4 - Querying the Database</span></span>

<span data-ttu-id="be9d3-313">W tym zadaniu zostanie zaktualizowana Klasa StoreController, tak aby zamiast korzystania z danych stałe była pobierana z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="be9d3-313">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will retrieve it from the database.</span></span>

> [!NOTE]
> <span data-ttu-id="be9d3-314">To zadanie jest wspólne w ćwiczeniu 1.</span><span class="sxs-lookup"><span data-stu-id="be9d3-314">This task is in common with Exercise 1.</span></span>
> 
> <span data-ttu-id="be9d3-315">Jeśli wykonasz Ćwiczenie 1, te kroki są takie same w obu podejściach (najpierw baza danych lub kod w pierwszej kolejności).</span><span class="sxs-lookup"><span data-stu-id="be9d3-315">If you completed Exercise 1 you will note these steps are the same in both approaches (Database first or Code first).</span></span> <span data-ttu-id="be9d3-316">Różnią się one sposobem, w jaki dane są połączone z modelem, ale dostęp do jednostek danych jest jeszcze niewidoczny dla kontrolera.</span><span class="sxs-lookup"><span data-stu-id="be9d3-316">They are different in how the data is linked with the model, but the access to data entities is yet transparent from the controller.</span></span>

1. <span data-ttu-id="be9d3-317">Otwórz **Controllers\StoreController.cs** i Dodaj następujące pole do klasy, aby pomieścić wystąpienie klasy **MusicStoreEntities** o nazwie **storeDB**:</span><span class="sxs-lookup"><span data-stu-id="be9d3-317">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="be9d3-318">(Fragment kodu — *modele i dostęp do danych — Ex1 storeDB*)</span><span class="sxs-lookup"><span data-stu-id="be9d3-318">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
2. <span data-ttu-id="be9d3-319">Klasa **MusicStoreEntities** uwidacznia Właściwość kolekcji dla każdej tabeli w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="be9d3-319">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="be9d3-320">Aktualizowanie metody akcji **przeglądania** w celu pobrania gatunku z wszystkimi **albumami**.</span><span class="sxs-lookup"><span data-stu-id="be9d3-320">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="be9d3-321">(Fragment kodu — *modele i dostęp do danych — Ex2 Store Browse*)</span><span class="sxs-lookup"><span data-stu-id="be9d3-321">(Code Snippet - *Models And Data Access - Ex2 Store Browse*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="be9d3-322">Używasz możliwości platformy .NET o nazwie **LINQ** (załączonej do języka) do pisania wyrażeń zapytania o jednoznacznie określonym typie względem tych kolekcji, które będą wykonywać kod względem bazy danych i zwracać obiekty, dla których można programować.</span><span class="sxs-lookup"><span data-stu-id="be9d3-322">You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.</span></span>
    > 
    > <span data-ttu-id="be9d3-323">Więcej informacji na temat programu LINQ można znaleźć w [witrynie MSDN](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="be9d3-323">For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).</span></span>
3. <span data-ttu-id="be9d3-324">Zaktualizuj metodę akcji **indeksu** w celu pobrania wszystkich gatunków.</span><span class="sxs-lookup"><span data-stu-id="be9d3-324">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="be9d3-325">(Fragment kodu — *modele i dostęp do danych — indeks magazynu Ex2*)</span><span class="sxs-lookup"><span data-stu-id="be9d3-325">(Code Snippet - *Models And Data Access - Ex2 Store Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
4. <span data-ttu-id="be9d3-326">Zaktualizuj metodę akcji **indeksu** w celu pobrania wszystkich gatunków i przekształcenia kolekcji na listę.</span><span class="sxs-lookup"><span data-stu-id="be9d3-326">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="be9d3-327">(Fragment kodu — *modele i dostęp do danych — Ex2 Store GenreMenu*)</span><span class="sxs-lookup"><span data-stu-id="be9d3-327">(Code Snippet - *Models And Data Access - Ex2 Store GenreMenu*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="be9d3-328">Zadanie 5 — Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="be9d3-328">Task 5 - Running the Application</span></span>

<span data-ttu-id="be9d3-329">W tym zadaniu sprawdzisz, że na stronie indeks sklepu będą teraz wyświetlane gatunki przechowywane w bazie danych zamiast stałe.</span><span class="sxs-lookup"><span data-stu-id="be9d3-329">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="be9d3-330">Nie ma potrzeby zmiany szablonu widoku, ponieważ **StoreController** zwraca taką samą **StoreIndexViewModel** jak poprzednio, ale ten czas danych pochodzi z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="be9d3-330">There is no need to change the View template because the **StoreController** is returning the same **StoreIndexViewModel** as before, but this time the data will come from the database.</span></span>

1. <span data-ttu-id="be9d3-331">Skompiluj ponownie rozwiązanie i naciśnij klawisz **F5** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="be9d3-331">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="be9d3-332">Projekt zostanie uruchomiony na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="be9d3-332">The project starts in the Home page.</span></span> <span data-ttu-id="be9d3-333">Sprawdź, czy menu **gatunek** nie jest już listą stałe, a dane są bezpośrednio pobierane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="be9d3-333">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    <span data-ttu-id="be9d3-335">*Przeglądanie gatunków z bazy danych*</span><span class="sxs-lookup"><span data-stu-id="be9d3-335">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="be9d3-336">Teraz przejdź do dowolnego gatunku i sprawdź, czy albumy są wypełnione z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="be9d3-336">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="be9d3-337">![Przeglądanie albumów z bazy danych](aspnet-mvc-4-models-and-data-access/_static/image23.png "Przeglądanie albumów z bazy danych")</span><span class="sxs-lookup"><span data-stu-id="be9d3-337">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image23.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="be9d3-338">*Przeglądanie albumów z bazy danych*</span><span class="sxs-lookup"><span data-stu-id="be9d3-338">*Browsing Albums from the database*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a><span data-ttu-id="be9d3-339">Ćwiczenie 3: wykonywanie zapytania dotyczącego bazy danych za pomocą parametrów</span><span class="sxs-lookup"><span data-stu-id="be9d3-339">Exercise 3: Querying the Database with Parameters</span></span>

<span data-ttu-id="be9d3-340">W tym ćwiczeniu dowiesz się, jak wysyłać zapytania do bazy danych przy użyciu parametrów i jak używać kształtowania wyników zapytania, a funkcja, która zmniejsza liczbę baz danych, uzyskuje dostęp do danych w bardziej wydajny sposób.</span><span class="sxs-lookup"><span data-stu-id="be9d3-340">In this exercise, you will learn how to query the database using parameters, and how to use Query Result Shaping, a feature that reduces the number database accesses retrieving data in a more efficient way.</span></span>

> [!NOTE]
> <span data-ttu-id="be9d3-341">Więcej informacji o kształtach wyników zapytania można znaleźć w następującym [artykule w witrynie MSDN](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="be9d3-341">For further information on Query Result Shaping, visit the following [msdn article](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a><span data-ttu-id="be9d3-342">Zadanie 1 — modyfikowanie StoreController w celu pobrania albumów z bazy danych</span><span class="sxs-lookup"><span data-stu-id="be9d3-342">Task 1 - Modifying StoreController to Retrieve Albums from Database</span></span>

<span data-ttu-id="be9d3-343">W tym zadaniu zmienimy klasę **StoreController** , aby uzyskać dostęp do bazy danych w celu pobrania albumów z określonego gatunku.</span><span class="sxs-lookup"><span data-stu-id="be9d3-343">In this task, you will change the **StoreController** class to access the database to retrieve albums from a specific genre.</span></span>

1. <span data-ttu-id="be9d3-344">Otwórz rozwiązanie **BEGIN** znajdujące się w folderze **Source\Ex3-QueryingTheDatabaseWithParametersCodeFirst\Begin** , jeśli chcesz użyć podejścia najpierw do kodu lub folderu **Source\Ex3-QueryingTheDatabaseWithParametersDBFirst\Begin** , jeśli chcesz użyć podejścia pierwszej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="be9d3-344">Open the **Begin** solution located at the **Source\Ex3-QueryingTheDatabaseWithParametersCodeFirst\Begin** folder if you want to use Code-First approach or **Source\Ex3-QueryingTheDatabaseWithParametersDBFirst\Begin** folder if you want to use Database-First approach.</span></span> <span data-ttu-id="be9d3-345">W przeciwnym razie można nadal korzystać z rozwiązania **końcowego** uzyskanego przez zakończenie poprzedniego ćwiczenia.</span><span class="sxs-lookup"><span data-stu-id="be9d3-345">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="be9d3-346">Jeśli otwarto **udostępnione rozwiązanie** , należy pobrać brakujące pakiety NuGet przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="be9d3-346">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="be9d3-347">W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="be9d3-347">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="be9d3-348">W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.</span><span class="sxs-lookup"><span data-stu-id="be9d3-348">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="be9d3-349">Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="be9d3-349">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="be9d3-350">Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="be9d3-350">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="be9d3-351">Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu.</span><span class="sxs-lookup"><span data-stu-id="be9d3-351">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="be9d3-352">To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="be9d3-352">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="be9d3-353">Otwórz klasę **StoreController** , aby zmienić metodę **przeglądania** akcji.</span><span class="sxs-lookup"><span data-stu-id="be9d3-353">Open the **StoreController** class to change the **Browse** action method.</span></span> <span data-ttu-id="be9d3-354">W tym celu w **Eksplorator rozwiązań**rozwiń folder **Kontrolery** i kliknij dwukrotnie pozycję **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="be9d3-354">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
3. <span data-ttu-id="be9d3-355">Zmień metodę akcji **Przeglądaj** , aby pobrać albumy dla określonego gatunku.</span><span class="sxs-lookup"><span data-stu-id="be9d3-355">Change the **Browse** action method to retrieve albums for a specific genre.</span></span> <span data-ttu-id="be9d3-356">Aby to zrobić, Zastąp następujący kod:</span><span class="sxs-lookup"><span data-stu-id="be9d3-356">To do this, replace the following code:</span></span>

    <span data-ttu-id="be9d3-357">(Fragment kodu — *modele i dostęp do danych — Ex3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="be9d3-357">(Code Snippet - *Models And Data Access - Ex3 StoreController BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

> [!NOTE]
> <span data-ttu-id="be9d3-358">Aby wypełnić kolekcję jednostki, musisz użyć metody **include** , aby określić, że chcesz pobrać albumy.</span><span class="sxs-lookup"><span data-stu-id="be9d3-358">To populate a collection of the entity, you need to use the **Include** method to specify you want to retrieve the albums too.</span></span> <span data-ttu-id="be9d3-359">Możesz użyć. Rozszerzenie **Single ()** w LINQ, ponieważ w tym przypadku oczekiwany jest tylko jeden gatunek dla albumu.</span><span class="sxs-lookup"><span data-stu-id="be9d3-359">You can use the .**Single()** extension in LINQ because in this case only one genre is expected for an album.</span></span> <span data-ttu-id="be9d3-360">**Pojedyncza Metoda ()** przyjmuje wyrażenie lambda jako parametr, który w tym przypadku określa pojedynczy obiekt gatunku, tak że jego nazwa pasuje do zdefiniowanej wartości.</span><span class="sxs-lookup"><span data-stu-id="be9d3-360">The **Single()** method takes a Lambda expression as a parameter, which in this case specifies a single Genre object such that its name matches the value defined.</span></span>
> 
> <span data-ttu-id="be9d3-361">Będziesz korzystać z funkcji, która pozwala wskazać inne powiązane jednostki, które mają być załadowane, a także po pobraniu obiektu gatunku.</span><span class="sxs-lookup"><span data-stu-id="be9d3-361">You will take advantage of a feature that allows you to indicate other related entities you want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="be9d3-362">Ta funkcja jest nazywana **kształtem wyników zapytania**i umożliwia zredukowanie liczby prób uzyskania dostępu do bazy danych w celu pobrania informacji.</span><span class="sxs-lookup"><span data-stu-id="be9d3-362">This feature is called **Query Result Shaping**, and enables you to reduce the number of times needed to access the database to retrieve information.</span></span> <span data-ttu-id="be9d3-363">W tym scenariuszu warto wstępnie pobrać albumy dla danego gatunku.</span><span class="sxs-lookup"><span data-stu-id="be9d3-363">In this scenario, you will want to pre-fetch the Albums for the Genre you retrieve.</span></span>
> 
> <span data-ttu-id="be9d3-364">Zapytanie zawiera **gatunek. Dołącz (&quot;albumy&quot;)** , aby wskazać, że chcesz również pokrewnych albumów.</span><span class="sxs-lookup"><span data-stu-id="be9d3-364">The query includes **Genres.Include(&quot;Albums&quot;)** to indicate that you want related albums as well.</span></span> <span data-ttu-id="be9d3-365">Spowoduje to wydajniejszą aplikację, ponieważ spowoduje pobieranie zarówno danych o gatunku, jak i albumie w ramach pojedynczego żądania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="be9d3-365">This will result in a more efficient application, since it will retrieve both Genre and Album data in a single database request.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="be9d3-366">Zadanie 2 — Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="be9d3-366">Task 2 - Running the Application</span></span>

<span data-ttu-id="be9d3-367">W tym zadaniu uruchomisz aplikację i pobierzesz albumy określonego gatunku z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="be9d3-367">In this task, you will run the application and retrieve albums of a specific genre from the database.</span></span>

1. <span data-ttu-id="be9d3-368">Naciśnij klawisz **F5** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="be9d3-368">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="be9d3-369">Projekt zostanie uruchomiony na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="be9d3-369">The project starts in the Home page.</span></span> <span data-ttu-id="be9d3-370">Zmień adres URL na **/Store/Browse? gatunek = pop** , aby sprawdzić, czy wyniki są pobierane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="be9d3-370">Change the URL to **/Store/Browse?genre=Pop** to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="be9d3-371">![Przeglądanie według gatunku](aspnet-mvc-4-models-and-data-access/_static/image24.png "Przeglądanie według gatunku")</span><span class="sxs-lookup"><span data-stu-id="be9d3-371">![Browsing by genre](aspnet-mvc-4-models-and-data-access/_static/image24.png "Browsing by genre")</span></span>

    <span data-ttu-id="be9d3-372">*Przeglądanie/Store/Browse? gatunek = pop*</span><span class="sxs-lookup"><span data-stu-id="be9d3-372">*Browsing /Store/Browse?genre=Pop*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a><span data-ttu-id="be9d3-373">Zadanie 3 — dostęp do albumów według identyfikatora</span><span class="sxs-lookup"><span data-stu-id="be9d3-373">Task 3 - Accessing Albums by Id</span></span>

<span data-ttu-id="be9d3-374">W tym zadaniu ponowimy poprzednią procedurę, aby uzyskać albumy według ich identyfikatorów.</span><span class="sxs-lookup"><span data-stu-id="be9d3-374">In this task, you will repeat the previous procedure to get albums by their Id.</span></span>

1. <span data-ttu-id="be9d3-375">W razie potrzeby Zamknij przeglądarkę, aby powrócić do programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="be9d3-375">Close the browser if needed, to return to Visual Studio.</span></span> <span data-ttu-id="be9d3-376">Otwórz klasę **StoreController** , aby zmienić metodę akcji **Details** .</span><span class="sxs-lookup"><span data-stu-id="be9d3-376">Open the **StoreController** class to change the **Details** action method.</span></span> <span data-ttu-id="be9d3-377">W tym celu w **Eksplorator rozwiązań**rozwiń folder **Kontrolery** i kliknij dwukrotnie pozycję **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="be9d3-377">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
2. <span data-ttu-id="be9d3-378">Zmień metodę akcji **Details** , aby pobrać szczegóły dotyczące albumów w oparciu o ich **identyfikatory**. Aby to zrobić, Zastąp następujący kod:</span><span class="sxs-lookup"><span data-stu-id="be9d3-378">Change the **Details** action method to retrieve albums details based on their **Id**. To do this, replace the following code:</span></span>

    <span data-ttu-id="be9d3-379">(Fragment kodu — *modele i dostęp do danych — Ex3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="be9d3-379">(Code Snippet - *Models And Data Access - Ex3 StoreController DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="be9d3-380">Zadanie 4 — Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="be9d3-380">Task 4 - Running the Application</span></span>

<span data-ttu-id="be9d3-381">W tym zadaniu aplikacja zostanie uruchomiona w przeglądarce internetowej i uzyska szczegóły dotyczące albumu według ich identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="be9d3-381">In this task, you will run the Application in a web browser and obtain album details by their Id.</span></span>

1. <span data-ttu-id="be9d3-382">Naciśnij klawisz **F5** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="be9d3-382">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="be9d3-383">Projekt zostanie uruchomiony na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="be9d3-383">The project starts in the Home page.</span></span> <span data-ttu-id="be9d3-384">Zmień adres URL na **/Store/Details/51** lub Przeglądaj gatunek i wybierz album, aby sprawdzić, czy wyniki są pobierane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="be9d3-384">Change the URL to **/Store/Details/51** or browse the genres and select an album to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="be9d3-385">![Szczegóły przeglądania](aspnet-mvc-4-models-and-data-access/_static/image25.png "Szczegóły przeglądania")</span><span class="sxs-lookup"><span data-stu-id="be9d3-385">![Browsing Details](aspnet-mvc-4-models-and-data-access/_static/image25.png "Browsing Details")</span></span>

    <span data-ttu-id="be9d3-386">*Przeglądanie/Store/Details/51*</span><span class="sxs-lookup"><span data-stu-id="be9d3-386">*Browsing /Store/Details/51*</span></span>

> [!NOTE]
> <span data-ttu-id="be9d3-387">Ponadto tę aplikację można wdrożyć w witrynach sieci Web systemu Windows Azure, które zostały opisane w [dodatku B: publikowanie aplikacji ASP.NET MVC 4 przy użyciu Web Deploy](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="be9d3-387">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="be9d3-388">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="be9d3-388">Summary</span></span>

<span data-ttu-id="be9d3-389">Wypełniając te praktyczne laboratorium, uczysz się podstaw modeli ASP.NET MVC i dostępu do danych, korzystając z podejścia **Database First** , a także podejścia **Code Firstowego** :</span><span class="sxs-lookup"><span data-stu-id="be9d3-389">By completing this Hands-on Lab you have learned the fundamentals of ASP.NET MVC Models and Data Access, using the **Database First** approach as well as the **Code First** Approach:</span></span>

- <span data-ttu-id="be9d3-390">Jak dodać bazę danych do rozwiązania w celu korzystania z jego danych</span><span class="sxs-lookup"><span data-stu-id="be9d3-390">How to add a database to the solution in order to consume its data</span></span>
- <span data-ttu-id="be9d3-391">Jak zaktualizować kontrolery, aby udostępnić szablony widoków z danymi pobranymi z bazy danych zamiast na stałe.</span><span class="sxs-lookup"><span data-stu-id="be9d3-391">How to update Controllers to provide View templates with the data taken from the database instead of hard-coded one</span></span>
- <span data-ttu-id="be9d3-392">Jak wysyłać zapytania do bazy danych przy użyciu parametrów</span><span class="sxs-lookup"><span data-stu-id="be9d3-392">How to query the database using parameters</span></span>
- <span data-ttu-id="be9d3-393">Jak używać kształtu wyniku zapytania, funkcji, która zmniejsza liczbę dostępu do bazy danych, pobierając dane w bardziej wydajny sposób</span><span class="sxs-lookup"><span data-stu-id="be9d3-393">How to use the Query Result Shaping, a feature that reduces the number of database accesses, retrieving data in a more efficient way</span></span>
- <span data-ttu-id="be9d3-394">Jak używać podejścia Database First i Code First w programie Microsoft Entity Framework do łączenia bazy danych z modelem</span><span class="sxs-lookup"><span data-stu-id="be9d3-394">How to use both Database First and Code First approaches in Microsoft Entity Framework to link the database with the model</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="be9d3-395">Dodatek A: Instalowanie Visual Studio Express 2012 dla sieci Web</span><span class="sxs-lookup"><span data-stu-id="be9d3-395">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="be9d3-396">Można zainstalować **Microsoft Visual Studio Express 2012 dla sieci Web** lub innej &quot;Express&quot; wersji za pomocą **[Instalator platformy Microsoft Web](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="be9d3-396">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="be9d3-397">Poniższe instrukcje przeprowadzą Cię przez kroki wymagane do zainstalowania *programu Visual Studio Express 2012 for Web* przy użyciu *Instalator platformy Microsoft Web*.</span><span class="sxs-lookup"><span data-stu-id="be9d3-397">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="be9d3-398">Przejdź do [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="be9d3-398">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="be9d3-399">Alternatywnie, jeśli zainstalowano już Instalatora platformy sieci Web, można go otworzyć i wyszukać produkt &quot;<em>Visual Studio Express 2012 for Web przy użyciu zestawu Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="be9d3-399">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="be9d3-400">Kliknij pozycję **Zainstaluj teraz**.</span><span class="sxs-lookup"><span data-stu-id="be9d3-400">Click on **Install Now**.</span></span> <span data-ttu-id="be9d3-401">Jeśli nie masz **Instalatora platformy sieci Web** , nastąpi przekierowanie do pobrania i zainstalowania go w pierwszej kolejności.</span><span class="sxs-lookup"><span data-stu-id="be9d3-401">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="be9d3-402">Po otwarciu **Instalatora platformy sieci Web** kliknij przycisk **Zainstaluj** , aby rozpocząć instalację.</span><span class="sxs-lookup"><span data-stu-id="be9d3-402">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="be9d3-403">![Zainstaluj Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "Zainstaluj Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="be9d3-403">![Install Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="be9d3-404">*Zainstaluj Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="be9d3-404">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="be9d3-405">Przeczytaj wszystkie licencje i postanowienia produktów, a następnie kliknij przycisk **Akceptuję** , aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="be9d3-405">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akceptowanie postanowień licencyjnych](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    <span data-ttu-id="be9d3-407">*Akceptowanie postanowień licencyjnych*</span><span class="sxs-lookup"><span data-stu-id="be9d3-407">*Accepting the license terms*</span></span>
5. <span data-ttu-id="be9d3-408">Poczekaj na zakończenie procesu pobierania i instalacji.</span><span class="sxs-lookup"><span data-stu-id="be9d3-408">Wait until the downloading and installation process completes.</span></span>

    ![Postęp instalacji](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    <span data-ttu-id="be9d3-410">*Postęp instalacji*</span><span class="sxs-lookup"><span data-stu-id="be9d3-410">*Installation progress*</span></span>
6. <span data-ttu-id="be9d3-411">Po zakończeniu instalacji kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="be9d3-411">When the installation completes, click **Finish**.</span></span>

    ![Instalacja zakończona](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    <span data-ttu-id="be9d3-413">*Instalacja zakończona*</span><span class="sxs-lookup"><span data-stu-id="be9d3-413">*Installation completed*</span></span>
7. <span data-ttu-id="be9d3-414">Kliknij przycisk **Zakończ** , aby zamknąć Instalatora platformy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="be9d3-414">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="be9d3-415">Aby otworzyć Visual Studio Express dla sieci Web, przejdź do ekranu **startowego** i zacznij pisać &quot;**vs Express**&quot;, a następnie kliknij kafelek **vs Express for Web** .</span><span class="sxs-lookup"><span data-stu-id="be9d3-415">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Kafelek VS Express for Web](aspnet-mvc-4-models-and-data-access/_static/image30.png)

    <span data-ttu-id="be9d3-417">*Kafelek VS Express for Web*</span><span class="sxs-lookup"><span data-stu-id="be9d3-417">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="be9d3-418">Dodatek B: publikowanie aplikacji ASP.NET MVC 4 przy użyciu Web Deploy</span><span class="sxs-lookup"><span data-stu-id="be9d3-418">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="be9d3-419">W tym dodatku pokazano, jak utworzyć nową witrynę sieci Web na podstawie portal zarządzania systemu Windows Azure i opublikować aplikację uzyskaną w wyniku działania laboratorium, korzystając z funkcji publikowania Web Deploy dostępnej w systemie Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="be9d3-419">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="be9d3-420">Zadanie 1 — Tworzenie nowej witryny sieci Web z poziomu portalu systemu Windows Azure</span><span class="sxs-lookup"><span data-stu-id="be9d3-420">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="be9d3-421">Przejdź do [Portal zarządzania systemu Windows Azure](https://manage.windowsazure.com/) i zaloguj się przy użyciu poświadczeń firmy Microsoft skojarzonych z Twoją subskrypcją.</span><span class="sxs-lookup"><span data-stu-id="be9d3-421">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="be9d3-422">System Windows Azure umożliwia bezpłatne hostowanie witryn sieci Web 10 ASP.NET, a następnie skalowanie ich w miarę wzrostu ruchu.</span><span class="sxs-lookup"><span data-stu-id="be9d3-422">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="be9d3-423">Możesz utworzyć konto w [tym miejscu](https://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="be9d3-423">You can sign up [here](https://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="be9d3-424">![Zaloguj się do systemu Windows Azure Portal](aspnet-mvc-4-models-and-data-access/_static/image31.png "Zaloguj się do systemu Windows Azure Portal")</span><span class="sxs-lookup"><span data-stu-id="be9d3-424">![Log on to Windows Azure portal](aspnet-mvc-4-models-and-data-access/_static/image31.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="be9d3-425">*Zaloguj się do usługi Windows Azure portal zarządzania*</span><span class="sxs-lookup"><span data-stu-id="be9d3-425">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="be9d3-426">Na pasku poleceń kliknij pozycję **Nowy** .</span><span class="sxs-lookup"><span data-stu-id="be9d3-426">Click **New** on the command bar.</span></span>

    <span data-ttu-id="be9d3-427">![Tworzenie nowej witryny sieci Web](aspnet-mvc-4-models-and-data-access/_static/image32.png "Tworzenie nowej witryny sieci Web")</span><span class="sxs-lookup"><span data-stu-id="be9d3-427">![Creating a new Web Site](aspnet-mvc-4-models-and-data-access/_static/image32.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="be9d3-428">*Tworzenie nowej witryny sieci Web*</span><span class="sxs-lookup"><span data-stu-id="be9d3-428">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="be9d3-429">Kliknij pozycję **obliczeniowe** | **witrynie sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="be9d3-429">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="be9d3-430">Następnie wybierz opcję **szybkie tworzenie** .</span><span class="sxs-lookup"><span data-stu-id="be9d3-430">Then select **Quick Create** option.</span></span> <span data-ttu-id="be9d3-431">Podaj dostępny adres URL dla nowej witryny sieci Web i kliknij pozycję **Utwórz witrynę sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="be9d3-431">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="be9d3-432">Witryna sieci Web systemu Windows Azure jest hostem aplikacji sieci Web działającej w chmurze, którą można kontrolować i zarządzać nią.</span><span class="sxs-lookup"><span data-stu-id="be9d3-432">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="be9d3-433">Opcja szybkie tworzenie umożliwia wdrożenie ukończonej aplikacji sieci Web w witrynie sieci Web systemu Windows Azure spoza portalu.</span><span class="sxs-lookup"><span data-stu-id="be9d3-433">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="be9d3-434">Nie obejmuje to kroków związanych z konfigurowaniem bazy danych.</span><span class="sxs-lookup"><span data-stu-id="be9d3-434">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="be9d3-435">![Tworzenie nowej witryny sieci Web przy użyciu funkcji szybkiego tworzenia](aspnet-mvc-4-models-and-data-access/_static/image33.png "Tworzenie nowej witryny sieci Web przy użyciu funkcji szybkiego tworzenia")</span><span class="sxs-lookup"><span data-stu-id="be9d3-435">![Creating a new Web Site using Quick Create](aspnet-mvc-4-models-and-data-access/_static/image33.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="be9d3-436">*Tworzenie nowej witryny sieci Web przy użyciu funkcji szybkiego tworzenia*</span><span class="sxs-lookup"><span data-stu-id="be9d3-436">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="be9d3-437">Poczekaj na utworzenie nowej **witryny sieci Web** .</span><span class="sxs-lookup"><span data-stu-id="be9d3-437">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="be9d3-438">Po utworzeniu witryny sieci Web kliknij link pod kolumną **adres URL** .</span><span class="sxs-lookup"><span data-stu-id="be9d3-438">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="be9d3-439">Sprawdź, czy nowa witryna sieci Web działa.</span><span class="sxs-lookup"><span data-stu-id="be9d3-439">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="be9d3-440">![Przeglądanie w nowej witrynie sieci Web](aspnet-mvc-4-models-and-data-access/_static/image34.png "Przeglądanie w nowej witrynie sieci Web")</span><span class="sxs-lookup"><span data-stu-id="be9d3-440">![Browsing to the new web site](aspnet-mvc-4-models-and-data-access/_static/image34.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="be9d3-441">*Przeglądanie w nowej witrynie sieci Web*</span><span class="sxs-lookup"><span data-stu-id="be9d3-441">*Browsing to the new web site*</span></span>

    <span data-ttu-id="be9d3-442">![Uruchomiona witryna sieci Web](aspnet-mvc-4-models-and-data-access/_static/image35.png "Uruchomiona witryna sieci Web")</span><span class="sxs-lookup"><span data-stu-id="be9d3-442">![Web site running](aspnet-mvc-4-models-and-data-access/_static/image35.png "Web site running")</span></span>

    <span data-ttu-id="be9d3-443">*Uruchomiona witryna sieci Web*</span><span class="sxs-lookup"><span data-stu-id="be9d3-443">*Web site running*</span></span>
6. <span data-ttu-id="be9d3-444">Wróć do portalu i kliknij nazwę witryny sieci Web pod kolumną **Nazwa** , aby wyświetlić strony zarządzania.</span><span class="sxs-lookup"><span data-stu-id="be9d3-444">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="be9d3-445">![Otwieranie stron zarządzania witryną sieci Web](aspnet-mvc-4-models-and-data-access/_static/image36.png "Otwieranie stron zarządzania witryną sieci Web")</span><span class="sxs-lookup"><span data-stu-id="be9d3-445">![Opening the web site management pages](aspnet-mvc-4-models-and-data-access/_static/image36.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="be9d3-446">*Otwieranie stron zarządzania witryną sieci Web*</span><span class="sxs-lookup"><span data-stu-id="be9d3-446">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="be9d3-447">Na stronie **pulpit nawigacyjny** w sekcji **szybki przegląd** kliknij link **Pobierz profil publikowania** .</span><span class="sxs-lookup"><span data-stu-id="be9d3-447">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="be9d3-448">*Profil publikowania* zawiera wszystkie informacje wymagane do opublikowania aplikacji sieci Web w witrynie sieci Web systemu Windows Azure dla każdej z włączonych metod publikacji.</span><span class="sxs-lookup"><span data-stu-id="be9d3-448">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="be9d3-449">Profil publikowania zawiera adresy URL, poświadczenia użytkownika i ciągi bazy danych wymagane do nawiązania połączenia i uwierzytelniania dla każdego punktu końcowego, dla którego jest włączona metoda publikacji.</span><span class="sxs-lookup"><span data-stu-id="be9d3-449">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="be9d3-450">**Program Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** i **Microsoft Visual Studio 2012** obsługują odczytywanie profilów publikowania w celu zautomatyzowania konfiguracji tych programów do publikowania aplikacji sieci Web w usłudze Microsoft Azure Websites.</span><span class="sxs-lookup"><span data-stu-id="be9d3-450">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="be9d3-451">![Pobieranie profilu publikowania witryny sieci Web](aspnet-mvc-4-models-and-data-access/_static/image37.png "Pobieranie profilu publikowania witryny sieci Web")</span><span class="sxs-lookup"><span data-stu-id="be9d3-451">![Downloading the web site publish profile](aspnet-mvc-4-models-and-data-access/_static/image37.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="be9d3-452">*Pobieranie profilu publikowania witryny sieci Web*</span><span class="sxs-lookup"><span data-stu-id="be9d3-452">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="be9d3-453">Pobierz plik profilu publikowania do znanej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="be9d3-453">Download the publish profile file to a known location.</span></span> <span data-ttu-id="be9d3-454">W tym ćwiczeniu zobaczysz, jak używać tego pliku do publikowania aplikacji sieci Web w witrynach sieci Web systemu Windows Azure z programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="be9d3-454">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="be9d3-455">![Zapisywanie pliku profilu publikowania](aspnet-mvc-4-models-and-data-access/_static/image38.png "Zapisywanie profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="be9d3-455">![Saving the publish profile file](aspnet-mvc-4-models-and-data-access/_static/image38.png "Saving the publish profile")</span></span>

    <span data-ttu-id="be9d3-456">*Zapisywanie pliku profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="be9d3-456">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="be9d3-457">Zadanie 2 — Konfigurowanie serwera bazy danych</span><span class="sxs-lookup"><span data-stu-id="be9d3-457">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="be9d3-458">Jeśli aplikacja korzysta z baz danych SQL Server, należy utworzyć SQL Database serwer.</span><span class="sxs-lookup"><span data-stu-id="be9d3-458">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="be9d3-459">Jeśli chcesz wdrożyć prostą aplikację, która nie używa SQL Server można pominąć to zadanie.</span><span class="sxs-lookup"><span data-stu-id="be9d3-459">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="be9d3-460">Do przechowywania bazy danych aplikacji potrzebny jest serwer SQL Database.</span><span class="sxs-lookup"><span data-stu-id="be9d3-460">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="be9d3-461">Serwery SQL Database z subskrypcji można wyświetlić w portalu zarządzania systemu Windows Azure w obszarze **bazy danych SQL** | **serwery** | **pulpitu nawigacyjnego serwera**.</span><span class="sxs-lookup"><span data-stu-id="be9d3-461">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="be9d3-462">Jeśli nie masz utworzonego serwera, możesz go utworzyć przy użyciu przycisku **Dodaj** na pasku poleceń.</span><span class="sxs-lookup"><span data-stu-id="be9d3-462">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="be9d3-463">Zanotuj **nazwę serwera i adres URL, nazwę logowania administratora i hasło**, ponieważ zostaną one użyte w następnych zadaniach.</span><span class="sxs-lookup"><span data-stu-id="be9d3-463">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="be9d3-464">Nie Twórz jeszcze bazy danych, ponieważ zostanie ona utworzona na późniejszym etapie.</span><span class="sxs-lookup"><span data-stu-id="be9d3-464">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="be9d3-465">![Pulpit nawigacyjny serwera SQL Database](aspnet-mvc-4-models-and-data-access/_static/image39.png "Pulpit nawigacyjny serwera SQL Database")</span><span class="sxs-lookup"><span data-stu-id="be9d3-465">![SQL Database Server Dashboard](aspnet-mvc-4-models-and-data-access/_static/image39.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="be9d3-466">*Pulpit nawigacyjny serwera SQL Database*</span><span class="sxs-lookup"><span data-stu-id="be9d3-466">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="be9d3-467">W następnym zadaniu zostanie przetestowane połączenie z bazą danych z programu Visual Studio. z tego powodu należy uwzględnić lokalny adres IP na liście **dozwolonych adresów IP**serwera.</span><span class="sxs-lookup"><span data-stu-id="be9d3-467">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="be9d3-468">W tym celu kliknij pozycję **Konfiguruj**, wybierz adres IP z **bieżącego adresu IP klienta** i wklej go w polach tekstowych **początkowy adres IP** i **końcowy adres IP** , a następnie kliknij przycisk ![Dodaj-Client-IP-Address-OK-Button](aspnet-mvc-4-models-and-data-access/_static/image40.png).</span><span class="sxs-lookup"><span data-stu-id="be9d3-468">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) button.</span></span>

    ![Dodawanie adresu IP klienta](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    <span data-ttu-id="be9d3-470">*Dodawanie adresu IP klienta*</span><span class="sxs-lookup"><span data-stu-id="be9d3-470">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="be9d3-471">Po dodaniu **adresu IP klienta** do listy dozwolone adresy IP kliknij pozycję **Zapisz** , aby potwierdzić zmiany.</span><span class="sxs-lookup"><span data-stu-id="be9d3-471">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Potwierdź zmiany](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    <span data-ttu-id="be9d3-473">*Potwierdź zmiany*</span><span class="sxs-lookup"><span data-stu-id="be9d3-473">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="be9d3-474">Zadanie 3 — publikowanie aplikacji ASP.NET MVC 4 przy użyciu Web Deploy</span><span class="sxs-lookup"><span data-stu-id="be9d3-474">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="be9d3-475">Wróć do rozwiązania ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="be9d3-475">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="be9d3-476">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt witryny sieci Web i wybierz polecenie **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="be9d3-476">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="be9d3-477">![Publikowanie aplikacji](aspnet-mvc-4-models-and-data-access/_static/image43.png "Publikowanie aplikacji")</span><span class="sxs-lookup"><span data-stu-id="be9d3-477">![Publishing the Application](aspnet-mvc-4-models-and-data-access/_static/image43.png "Publishing the Application")</span></span>

    <span data-ttu-id="be9d3-478">*Publikowanie witryny sieci Web*</span><span class="sxs-lookup"><span data-stu-id="be9d3-478">*Publishing the web site*</span></span>
2. <span data-ttu-id="be9d3-479">Zaimportuj profil publikowania zapisany w pierwszym zadaniu.</span><span class="sxs-lookup"><span data-stu-id="be9d3-479">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="be9d3-480">![Importowanie profilu publikowania](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importowanie profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="be9d3-480">![Importing the publish profile](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importing the publish profile")</span></span>

    <span data-ttu-id="be9d3-481">*Importowanie profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="be9d3-481">*Importing publish profile*</span></span>
3. <span data-ttu-id="be9d3-482">Kliknij pozycję **Weryfikuj połączenie**.</span><span class="sxs-lookup"><span data-stu-id="be9d3-482">Click **Validate Connection**.</span></span> <span data-ttu-id="be9d3-483">Po zakończeniu walidacji kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="be9d3-483">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="be9d3-484">Walidacja jest ukończona po wyświetleniu zielonego znacznika wyboru obok przycisku Weryfikuj połączenie.</span><span class="sxs-lookup"><span data-stu-id="be9d3-484">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="be9d3-485">![Weryfikowanie połączenia](aspnet-mvc-4-models-and-data-access/_static/image45.png "Weryfikowanie połączenia")</span><span class="sxs-lookup"><span data-stu-id="be9d3-485">![Validating connection](aspnet-mvc-4-models-and-data-access/_static/image45.png "Validating connection")</span></span>

    <span data-ttu-id="be9d3-486">*Weryfikowanie połączenia*</span><span class="sxs-lookup"><span data-stu-id="be9d3-486">*Validating connection*</span></span>
4. <span data-ttu-id="be9d3-487">Na stronie **Ustawienia** w sekcji **bazy danych** kliknij przycisk obok pola tekstowego połączenie z bazą danych (np. **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="be9d3-487">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="be9d3-488">![Konfiguracja narzędzia Web Deploy](aspnet-mvc-4-models-and-data-access/_static/image46.png "Konfiguracja narzędzia Web Deploy")</span><span class="sxs-lookup"><span data-stu-id="be9d3-488">![Web deploy configuration](aspnet-mvc-4-models-and-data-access/_static/image46.png "Web deploy configuration")</span></span>

    <span data-ttu-id="be9d3-489">*Konfiguracja narzędzia Web Deploy*</span><span class="sxs-lookup"><span data-stu-id="be9d3-489">*Web deploy configuration*</span></span>
5. <span data-ttu-id="be9d3-490">Skonfiguruj połączenie z bazą danych w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="be9d3-490">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="be9d3-491">W polu **Nazwa serwera** wpisz adres URL serwera SQL Database przy użyciu prefiksu *TCP:* .</span><span class="sxs-lookup"><span data-stu-id="be9d3-491">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="be9d3-492">W polu **Nazwa użytkownika** wpisz nazwę logowania administratora serwera.</span><span class="sxs-lookup"><span data-stu-id="be9d3-492">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="be9d3-493">W polu **hasło** wpisz hasło logowania administratora serwera.</span><span class="sxs-lookup"><span data-stu-id="be9d3-493">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="be9d3-494">Wpisz nową nazwę bazy danych.</span><span class="sxs-lookup"><span data-stu-id="be9d3-494">Type a new database name.</span></span>

     <span data-ttu-id="be9d3-495">![Konfigurowanie parametrów połączenia docelowego](aspnet-mvc-4-models-and-data-access/_static/image47.png "Konfigurowanie parametrów połączenia docelowego")</span><span class="sxs-lookup"><span data-stu-id="be9d3-495">![Configuring destination connection string](aspnet-mvc-4-models-and-data-access/_static/image47.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="be9d3-496">*Konfigurowanie parametrów połączenia docelowego*</span><span class="sxs-lookup"><span data-stu-id="be9d3-496">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="be9d3-497">Następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="be9d3-497">Then click **OK**.</span></span> <span data-ttu-id="be9d3-498">Po wyświetleniu monitu o utworzenie bazy danych kliknij przycisk **tak**.</span><span class="sxs-lookup"><span data-stu-id="be9d3-498">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="be9d3-499">![Tworzenie bazy danych](aspnet-mvc-4-models-and-data-access/_static/image48.png "Tworzenie ciągu bazy danych")</span><span class="sxs-lookup"><span data-stu-id="be9d3-499">![Creating the database](aspnet-mvc-4-models-and-data-access/_static/image48.png "Creating the database string")</span></span>

    <span data-ttu-id="be9d3-500">*Tworzenie bazy danych*</span><span class="sxs-lookup"><span data-stu-id="be9d3-500">*Creating the database*</span></span>
7. <span data-ttu-id="be9d3-501">Parametry połączenia, które będą używane do nawiązywania połączenia z SQL Database w systemie Windows Azure, są wyświetlane w domyślnym polu tekstowym połączenie.</span><span class="sxs-lookup"><span data-stu-id="be9d3-501">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="be9d3-502">Następnie kliknij przycisk **Next** (Dalej).</span><span class="sxs-lookup"><span data-stu-id="be9d3-502">Then click **Next**.</span></span>

    <span data-ttu-id="be9d3-503">![Parametry połączenia wskazujące SQL Database](aspnet-mvc-4-models-and-data-access/_static/image49.png "Parametry połączenia wskazujące SQL Database")</span><span class="sxs-lookup"><span data-stu-id="be9d3-503">![Connection string pointing to SQL Database](aspnet-mvc-4-models-and-data-access/_static/image49.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="be9d3-504">*Parametry połączenia wskazujące SQL Database*</span><span class="sxs-lookup"><span data-stu-id="be9d3-504">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="be9d3-505">Na stronie **Podgląd** kliknij przycisk **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="be9d3-505">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="be9d3-506">![Publikowanie aplikacji sieci Web](aspnet-mvc-4-models-and-data-access/_static/image50.png "Publikowanie aplikacji sieci Web")</span><span class="sxs-lookup"><span data-stu-id="be9d3-506">![Publishing the web application](aspnet-mvc-4-models-and-data-access/_static/image50.png "Publishing the web application")</span></span>

    <span data-ttu-id="be9d3-507">*Publikowanie aplikacji sieci Web*</span><span class="sxs-lookup"><span data-stu-id="be9d3-507">*Publishing the web application*</span></span>
9. <span data-ttu-id="be9d3-508">Po zakończeniu procesu publikowania w domyślnej przeglądarce zostanie otwarta opublikowana witryna sieci Web.</span><span class="sxs-lookup"><span data-stu-id="be9d3-508">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="be9d3-509">Dodatek C: używanie fragmentów kodu</span><span class="sxs-lookup"><span data-stu-id="be9d3-509">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="be9d3-510">Za pomocą fragmentów kodu masz cały kod, którego potrzebujesz na wyręką.</span><span class="sxs-lookup"><span data-stu-id="be9d3-510">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="be9d3-511">Dokument laboratoryjny powiedzie się dokładnie wtedy, gdy można z nich korzystać, jak pokazano na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="be9d3-511">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="be9d3-512">![Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio](aspnet-mvc-4-models-and-data-access/_static/image51.png "Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio")</span><span class="sxs-lookup"><span data-stu-id="be9d3-512">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-models-and-data-access/_static/image51.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="be9d3-513">*Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="be9d3-513">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="be9d3-514">***Aby dodać fragment kodu przy użyciu klawiatury (C# tylko)***</span><span class="sxs-lookup"><span data-stu-id="be9d3-514">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="be9d3-515">Umieść kursor, w którym chcesz wstawić kod.</span><span class="sxs-lookup"><span data-stu-id="be9d3-515">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="be9d3-516">Zacznij wpisywać nazwę fragmentu kodu (bez spacji lub łączników).</span><span class="sxs-lookup"><span data-stu-id="be9d3-516">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="be9d3-517">Obejrzyj jako IntelliSense wyświetla pasujące nazwy fragmentów kodu.</span><span class="sxs-lookup"><span data-stu-id="be9d3-517">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="be9d3-518">Wybierz poprawny fragment kodu (lub Kontynuuj wpisywanie, dopóki nie zostanie zaznaczona cała nazwa fragmentu kodu).</span><span class="sxs-lookup"><span data-stu-id="be9d3-518">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="be9d3-519">Naciśnij dwukrotnie klawisz Tab, aby wstawić fragment kodu w lokalizacji kursora.</span><span class="sxs-lookup"><span data-stu-id="be9d3-519">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="be9d3-520">![Zacznij wpisywać nazwę fragmentu kodu](aspnet-mvc-4-models-and-data-access/_static/image52.png "Zacznij wpisywać nazwę fragmentu kodu")</span><span class="sxs-lookup"><span data-stu-id="be9d3-520">![Start typing the snippet name](aspnet-mvc-4-models-and-data-access/_static/image52.png "Start typing the snippet name")</span></span>

<span data-ttu-id="be9d3-521">*Zacznij wpisywać nazwę fragmentu kodu*</span><span class="sxs-lookup"><span data-stu-id="be9d3-521">*Start typing the snippet name*</span></span>

<span data-ttu-id="be9d3-522">![Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu](aspnet-mvc-4-models-and-data-access/_static/image53.png "Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu")</span><span class="sxs-lookup"><span data-stu-id="be9d3-522">![Press Tab to select the highlighted snippet](aspnet-mvc-4-models-and-data-access/_static/image53.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="be9d3-523">*Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="be9d3-523">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="be9d3-524">![Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta](aspnet-mvc-4-models-and-data-access/_static/image54.png "Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta")</span><span class="sxs-lookup"><span data-stu-id="be9d3-524">![Press Tab again and the snippet will expand](aspnet-mvc-4-models-and-data-access/_static/image54.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="be9d3-525">*Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta*</span><span class="sxs-lookup"><span data-stu-id="be9d3-525">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="be9d3-526">***Aby dodać fragment kodu przy użyciu myszy (C#, Visual Basic i XML)*** jedno.</span><span class="sxs-lookup"><span data-stu-id="be9d3-526">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="be9d3-527">Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu.</span><span class="sxs-lookup"><span data-stu-id="be9d3-527">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="be9d3-528">Wybierz **Wstaw fragment** kodu, po którym następuje **Moje fragmenty kodów**.</span><span class="sxs-lookup"><span data-stu-id="be9d3-528">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="be9d3-529">Wybierz odpowiedni fragment kodu z listy, klikając go.</span><span class="sxs-lookup"><span data-stu-id="be9d3-529">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="be9d3-530">![Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment](aspnet-mvc-4-models-and-data-access/_static/image55.png "Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment")</span><span class="sxs-lookup"><span data-stu-id="be9d3-530">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-models-and-data-access/_static/image55.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="be9d3-531">*Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment*</span><span class="sxs-lookup"><span data-stu-id="be9d3-531">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="be9d3-532">![Wybierz odpowiedni fragment kodu z listy, klikając go](aspnet-mvc-4-models-and-data-access/_static/image56.png "Wybierz odpowiedni fragment kodu z listy, klikając go")</span><span class="sxs-lookup"><span data-stu-id="be9d3-532">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-models-and-data-access/_static/image56.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="be9d3-533">*Wybierz odpowiedni fragment kodu z listy, klikając go*</span><span class="sxs-lookup"><span data-stu-id="be9d3-533">*Pick the relevant snippet from the list, by clicking on it*</span></span>
