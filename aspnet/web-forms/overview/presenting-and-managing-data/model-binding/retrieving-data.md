---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: Pobieranie i wyświetlanie danych przy użyciu powiązania modelu i formularzy sieci Web | Microsoft Docs
author: Rick-Anderson
description: Ta seria samouczków pokazuje podstawowe aspekty używania powiązania modelu z projektem formularzy sieci Web ASP.NET. Powiązanie modelu sprawia, że interakcje danych są bardziej proste-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: 81cca22cb4752d071d2a68986ae9ac2bed737594
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74633171"
---
# <a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a><span data-ttu-id="41712-104">Pobieranie i wyświetlanie danych przy użyciu powiązania modelu i formularzy sieci Web</span><span class="sxs-lookup"><span data-stu-id="41712-104">Retrieving and displaying data with model binding and web forms</span></span>

> <span data-ttu-id="41712-105">Ta seria samouczków pokazuje podstawowe aspekty używania powiązania modelu z projektem formularzy sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="41712-105">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="41712-106">Powiązanie modelu sprawia, że interakcje danych są bardziej proste, niż w przypadku obiektów źródła danych (np. ObjectDataSource lub kontrolki SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="41712-106">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="41712-107">Ta seria rozpoczyna się od materiału wstępnego i przenosi do bardziej zaawansowanych koncepcji w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="41712-107">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="41712-108">Wzorzec powiązania modelu współpracuje z dowolnymi technologiami dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="41712-108">The model binding pattern works with any data access technology.</span></span> <span data-ttu-id="41712-109">W tym samouczku będziesz używać Entity Framework, ale możesz korzystać z najpopularniejszej technologii dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="41712-109">In this tutorial, you will use Entity Framework, but you could use the data access technology that is most familiar to you.</span></span> <span data-ttu-id="41712-110">Z poziomu formantu serwera powiązanego z danymi, takiego jak formant GridView, ListView, DetailsView lub FormView, należy określić nazwy metod, które mają być używane do wybierania, aktualizowania, usuwania i tworzenia danych.</span><span class="sxs-lookup"><span data-stu-id="41712-110">From a data-bound server control, such as a GridView, ListView, DetailsView, or FormView control, you specify the names of the methods to use for selecting, updating, deleting, and creating data.</span></span> <span data-ttu-id="41712-111">W tym samouczku określisz wartość dla operacji SelectMethod.</span><span class="sxs-lookup"><span data-stu-id="41712-111">In this tutorial, you will specify a value for the SelectMethod.</span></span> 
> 
> <span data-ttu-id="41712-112">W ramach tej metody należy podać logikę pobierania danych.</span><span class="sxs-lookup"><span data-stu-id="41712-112">Within that method, you provide the logic for retrieving the data.</span></span> <span data-ttu-id="41712-113">W następnym samouczku ustawisz wartości dla UpdateMethod, DeleteMethod i InsertMethod.</span><span class="sxs-lookup"><span data-stu-id="41712-113">In the next tutorial, you will set values for UpdateMethod, DeleteMethod and InsertMethod.</span></span>
>
> <span data-ttu-id="41712-114">Możesz [pobrać](https://go.microsoft.com/fwlink/?LinkId=286116) kompletny projekt w programie C# lub Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="41712-114">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or Visual Basic.</span></span> <span data-ttu-id="41712-115">Kod do pobrania współpracuje z programem Visual Studio 2012 lub nowszym.</span><span class="sxs-lookup"><span data-stu-id="41712-115">The downloadable code works with Visual Studio 2012 and later.</span></span> <span data-ttu-id="41712-116">Używa szablonu programu Visual Studio 2012, który jest nieco inny niż szablon programu Visual Studio 2017 przedstawiony w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="41712-116">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2017 template shown in this tutorial.</span></span>
> 
> <span data-ttu-id="41712-117">Samouczek dotyczący uruchamiania aplikacji w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="41712-117">In the tutorial you run the application in Visual Studio.</span></span> <span data-ttu-id="41712-118">Możesz również wdrożyć aplikację w dostawcy hostingu i udostępnić ją za pośrednictwem Internetu.</span><span class="sxs-lookup"><span data-stu-id="41712-118">You can also deploy the application to a hosting provider and make it available over the internet.</span></span> <span data-ttu-id="41712-119">Firma Microsoft oferuje bezpłatny hosting w sieci Web dla maksymalnie 10 witryn sieci Web w</span><span class="sxs-lookup"><span data-stu-id="41712-119">Microsoft offers free web hosting for up to 10 web sites in a</span></span>  
> <span data-ttu-id="41712-120">[bezpłatne konto wersji próbnej platformy Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="41712-120">[free Azure trial account](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="41712-121">Aby uzyskać informacje na temat sposobu wdrażania projektu sieci Web programu Visual Studio w celu Azure App Service Web Apps, zobacz [wdrażanie w sieci web ASP.NET za pomocą programu Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) Series.</span><span class="sxs-lookup"><span data-stu-id="41712-121">For information about how to deploy a Visual Studio web project to Azure App Service Web Apps, see the [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) series.</span></span> <span data-ttu-id="41712-122">Ten samouczek pokazuje również, jak używać migracje Code First platformy Entity Framework do wdrażania bazy danych SQL Server do Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="41712-122">That tutorial also shows how to use Entity Framework Code First Migrations to deploy your SQL Server database to Azure SQL Database.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="41712-123">Wersje oprogramowania używane w samouczku</span><span class="sxs-lookup"><span data-stu-id="41712-123">Software versions used in the tutorial</span></span>
> 
> - <span data-ttu-id="41712-124">Microsoft Visual Studio 2017 lub Microsoft Visual Studio Community 2017</span><span class="sxs-lookup"><span data-stu-id="41712-124">Microsoft Visual Studio 2017 or Microsoft Visual Studio Community 2017</span></span>
>   
> <span data-ttu-id="41712-125">Ten samouczek działa również z programem Visual Studio 2012 i Visual Studio 2013, ale istnieją pewne różnice w interfejsie użytkownika i szablonie projektu.</span><span class="sxs-lookup"><span data-stu-id="41712-125">This tutorial also works with Visual Studio 2012 and Visual Studio 2013, but there are some differences in the user interface and project template.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="41712-126">Co będziesz kompilować</span><span class="sxs-lookup"><span data-stu-id="41712-126">What you'll build</span></span>

<span data-ttu-id="41712-127">W tym samouczku przedstawiono następujące instrukcje:</span><span class="sxs-lookup"><span data-stu-id="41712-127">In this tutorial, you'll:</span></span>

* <span data-ttu-id="41712-128">Kompiluj obiekty danych, które odzwierciedlają Uniwersytet z uczniami zarejestrowanymi w kursach</span><span class="sxs-lookup"><span data-stu-id="41712-128">Build data objects that reflect a university with students enrolled in courses</span></span>
* <span data-ttu-id="41712-129">Tworzenie tabel bazy danych z obiektów</span><span class="sxs-lookup"><span data-stu-id="41712-129">Build database tables from the objects</span></span>
* <span data-ttu-id="41712-130">Wypełnianie bazy danych danymi testowymi</span><span class="sxs-lookup"><span data-stu-id="41712-130">Populate the database with test data</span></span>
* <span data-ttu-id="41712-131">Wyświetlanie danych w formularzu sieci Web</span><span class="sxs-lookup"><span data-stu-id="41712-131">Display data in a web form</span></span>

## <a name="create-the-project"></a><span data-ttu-id="41712-132">Utwórz projekt</span><span class="sxs-lookup"><span data-stu-id="41712-132">Create the project</span></span>

1. <span data-ttu-id="41712-133">W programie Visual Studio 2017 Utwórz projekt **aplikacji sieci Web ASP.NET (.NET Framework)** o nazwie **ContosoUniversityModelBinding**.</span><span class="sxs-lookup"><span data-stu-id="41712-133">In Visual Studio 2017, create a **ASP.NET Web Application (.NET Framework)** project called **ContosoUniversityModelBinding**.</span></span>

   ![Utwórz projekt](retrieving-data/_static/image19.png)

2. <span data-ttu-id="41712-135">Wybierz **przycisk OK**.</span><span class="sxs-lookup"><span data-stu-id="41712-135">Select **OK**.</span></span> <span data-ttu-id="41712-136">Zostanie wyświetlone okno dialogowe z wybranym szablonem.</span><span class="sxs-lookup"><span data-stu-id="41712-136">The dialog box to select a template appears.</span></span>

   ![Wybieranie formularzy sieci Web](retrieving-data/_static/image3.png)

3. <span data-ttu-id="41712-138">Wybierz szablon **formularzy sieci Web** .</span><span class="sxs-lookup"><span data-stu-id="41712-138">Select the **Web Forms** template.</span></span> 

4. <span data-ttu-id="41712-139">W razie potrzeby zmień uwierzytelnianie na **konta poszczególnych użytkowników**.</span><span class="sxs-lookup"><span data-stu-id="41712-139">If necessary, change the authentication to **Individual User Accounts**.</span></span> 

5. <span data-ttu-id="41712-140">Wybierz **przycisk OK** , aby utworzyć projekt.</span><span class="sxs-lookup"><span data-stu-id="41712-140">Select **OK** to create the project.</span></span>

## <a name="modify-site-appearance"></a><span data-ttu-id="41712-141">Modyfikuj wygląd witryny</span><span class="sxs-lookup"><span data-stu-id="41712-141">Modify site appearance</span></span>

   <span data-ttu-id="41712-142">Wprowadź kilka zmian, aby dostosować wygląd witryny.</span><span class="sxs-lookup"><span data-stu-id="41712-142">Make a few changes to customize site appearance.</span></span> 
   
   1. <span data-ttu-id="41712-143">Otwórz plik site. Master.</span><span class="sxs-lookup"><span data-stu-id="41712-143">Open the Site.Master file.</span></span>
   
   2. <span data-ttu-id="41712-144">Zmień tytuł w taki sposób, aby wyświetlał firmę **Contoso University** , a nie **moją aplikację ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="41712-144">Change the title to display **Contoso University** and not **My ASP.NET Application**.</span></span>

      [!code-aspx-csharp[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

   3. <span data-ttu-id="41712-145">Zmień tekst nagłówka z **nazwy aplikacji** na **Uniwersytet firmy Contoso**.</span><span class="sxs-lookup"><span data-stu-id="41712-145">Change the header text from **Application name** to **Contoso University**.</span></span>

      [!code-aspx-csharp[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

   4. <span data-ttu-id="41712-146">Zmień linki nagłówka nawigacji odpowiednio do odpowiedniej lokacji.</span><span class="sxs-lookup"><span data-stu-id="41712-146">Change the navigation header links to site appropriate ones.</span></span> 
   
      <span data-ttu-id="41712-147">Usuń linki do programu i **skontaktuj się** z nim **, a zamiast** tego Połącz się ze stroną **uczniów** , którą utworzysz.</span><span class="sxs-lookup"><span data-stu-id="41712-147">Remove the links for **About** and **Contact** and, instead, link to a **Students** page, which you will create.</span></span>

      [!code-aspx-csharp[Main](retrieving-data/samples/sample3.aspx)]

   5. <span data-ttu-id="41712-148">Zapisz witrynę site. Master.</span><span class="sxs-lookup"><span data-stu-id="41712-148">Save Site.Master.</span></span>

## <a name="add-a-web-form-to-display-student-data"></a><span data-ttu-id="41712-149">Dodaj formularz sieci Web, aby wyświetlić dane ucznia</span><span class="sxs-lookup"><span data-stu-id="41712-149">Add a web form to display student data</span></span>

   1. <span data-ttu-id="41712-150">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt, wybierz polecenie **Dodaj** , a następnie **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="41712-150">In **Solution Explorer**, right-click your project, select **Add** and then **New Item**.</span></span> 
   
   2. <span data-ttu-id="41712-151">W oknie dialogowym **Dodaj nowy element** wybierz **formularz sieci Web z szablonem strony głównej** i nadaj mu nazwę **Students. aspx**.</span><span class="sxs-lookup"><span data-stu-id="41712-151">In the **Add New Item** dialog box, select the **Web Form with Master Page** template and name it **Students.aspx**.</span></span>

      ![Utwórz stronę](retrieving-data/_static/image5.png)

   3. <span data-ttu-id="41712-153">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="41712-153">Select **Add**.</span></span>
   
   4. <span data-ttu-id="41712-154">Na stronie wzorcowej formularza sieci Web wybierz pozycję **site. Master**.</span><span class="sxs-lookup"><span data-stu-id="41712-154">For the web form's master page, select **Site.Master**.</span></span>
   
   5. <span data-ttu-id="41712-155">Wybierz **przycisk OK**.</span><span class="sxs-lookup"><span data-stu-id="41712-155">Select **OK**.</span></span>

## <a name="add-the-data-model"></a><span data-ttu-id="41712-156">Dodawanie modelu danych</span><span class="sxs-lookup"><span data-stu-id="41712-156">Add the data model</span></span>

<span data-ttu-id="41712-157">W folderze **modele** Dodaj klasę o nazwie **UniversityModels.cs**.</span><span class="sxs-lookup"><span data-stu-id="41712-157">In the **Models** folder, add a class named **UniversityModels.cs**.</span></span>

   1. <span data-ttu-id="41712-158">Kliknij prawym przyciskiem myszy pozycję **modele**, wybierz pozycję **Dodaj**, a następnie pozycję **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="41712-158">Right-click **Models**, select **Add**, and then **New Item**.</span></span> <span data-ttu-id="41712-159">Pojawi się okno dialogowe **Dodaj nowy element** .</span><span class="sxs-lookup"><span data-stu-id="41712-159">The **Add New Item** dialog box appears.</span></span>

   2. <span data-ttu-id="41712-160">W menu nawigacji po lewej stronie wybierz pozycję **Code**, a następnie **Class**.</span><span class="sxs-lookup"><span data-stu-id="41712-160">From the left navigation menu, select **Code**, then **Class**.</span></span>

      ![Utwórz klasę modelu](retrieving-data/_static/image20.png)

   3. <span data-ttu-id="41712-162">Nadaj klasie nazwę **UniversityModels.cs** i wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="41712-162">Name the class **UniversityModels.cs** and select **Add**.</span></span>

      <span data-ttu-id="41712-163">W tym pliku Zdefiniuj klasy `SchoolContext`, `Student`, `Enrollment`i `Course` w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="41712-163">In this file, define the `SchoolContext`, `Student`, `Enrollment`, and `Course` classes as follows:</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample4.cs)]

      <span data-ttu-id="41712-164">Klasa `SchoolContext` pochodzi od `DbContext`, która zarządza połączeniem bazy danych i zmianami danych.</span><span class="sxs-lookup"><span data-stu-id="41712-164">The `SchoolContext` class derives from `DbContext`, which manages the database connection and changes in the data.</span></span>

      <span data-ttu-id="41712-165">W klasie `Student` Zwróć uwagę na atrybuty zastosowane do właściwości `FirstName`, `LastName`i `Year`.</span><span class="sxs-lookup"><span data-stu-id="41712-165">In the `Student` class, notice the attributes applied to the `FirstName`, `LastName`, and `Year` properties.</span></span> <span data-ttu-id="41712-166">Ten samouczek używa tych atrybutów do sprawdzania poprawności danych.</span><span class="sxs-lookup"><span data-stu-id="41712-166">This tutorial uses these attributes for data validation.</span></span> <span data-ttu-id="41712-167">Aby uprościć kod, tylko te właściwości są oznaczone atrybutami walidacji danych.</span><span class="sxs-lookup"><span data-stu-id="41712-167">To simplify the code, only these properties are marked with data-validation attributes.</span></span> <span data-ttu-id="41712-168">W rzeczywistym projekcie należy zastosować atrybuty walidacji do wszystkich właściwości wymagających walidacji.</span><span class="sxs-lookup"><span data-stu-id="41712-168">In a real project, you would apply validation attributes to all properties needing validation.</span></span>

   4. <span data-ttu-id="41712-169">Zapisz UniversityModels.cs.</span><span class="sxs-lookup"><span data-stu-id="41712-169">Save UniversityModels.cs.</span></span>

## <a name="set-up-the-database-based-on-classes"></a><span data-ttu-id="41712-170">Skonfiguruj bazę danych na podstawie klas</span><span class="sxs-lookup"><span data-stu-id="41712-170">Set up the database based on classes</span></span>

<span data-ttu-id="41712-171">Ten samouczek używa [migracje Code First](https://docs.microsoft.com/ef/ef6/modeling/code-first/migrations/) do tworzenia obiektów i tabel bazy danych.</span><span class="sxs-lookup"><span data-stu-id="41712-171">This tutorial uses [Code First Migrations](https://docs.microsoft.com/ef/ef6/modeling/code-first/migrations/) to create objects and database tables.</span></span> <span data-ttu-id="41712-172">Te tabele przechowują informacje dotyczące uczniów i ich kursów.</span><span class="sxs-lookup"><span data-stu-id="41712-172">These tables store information about the students and their courses.</span></span>

   1. <span data-ttu-id="41712-173">Wybierz kolejno pozycje **narzędzia** > **menedżer pakietów NuGet** > **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="41712-173">Select **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

   2. <span data-ttu-id="41712-174">W **konsoli Menedżera pakietów**Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="41712-174">In **Package Manager Console**, run this command:</span></span>  
      `enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

      <span data-ttu-id="41712-175">Jeśli polecenie zostanie wykonane pomyślnie, zostanie wyświetlony komunikat z informacją o włączeniu migracji.</span><span class="sxs-lookup"><span data-stu-id="41712-175">If the command completes successfully, a message stating migrations have been enabled appears.</span></span>

      ![Włącz migracje](retrieving-data/_static/image8.png)

      <span data-ttu-id="41712-177">Zauważ, że plik o nazwie *Configuration.cs* został utworzony.</span><span class="sxs-lookup"><span data-stu-id="41712-177">Notice that a file named *Configuration.cs* has been created.</span></span> <span data-ttu-id="41712-178">Klasa `Configuration` ma metodę `Seed`, która umożliwia wstępne wypełnianie tabel bazy danych danymi testowymi.</span><span class="sxs-lookup"><span data-stu-id="41712-178">The `Configuration` class has a `Seed` method, which can pre-populate the database tables with test data.</span></span>

## <a name="pre-populate-the-database"></a><span data-ttu-id="41712-179">Wstępne wypełnianie bazy danych</span><span class="sxs-lookup"><span data-stu-id="41712-179">Pre-populate the database</span></span>

   1. <span data-ttu-id="41712-180">Otwórz Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="41712-180">Open Configuration.cs.</span></span>
   
   2. <span data-ttu-id="41712-181">Dodaj następujący kod do metody `Seed`.</span><span class="sxs-lookup"><span data-stu-id="41712-181">Add the following code to the `Seed` method.</span></span> <span data-ttu-id="41712-182">Ponadto Dodaj instrukcję `using` dla przestrzeni nazw `ContosoUniversityModelBinding. Models`.</span><span class="sxs-lookup"><span data-stu-id="41712-182">Also, add a `using` statement for the `ContosoUniversityModelBinding. Models` namespace.</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample5.cs)]

   3. <span data-ttu-id="41712-183">Zapisz Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="41712-183">Save Configuration.cs.</span></span>

   4. <span data-ttu-id="41712-184">W konsoli Menedżera pakietów Uruchom polecenie **Dodaj wstępną migrację**polecenia.</span><span class="sxs-lookup"><span data-stu-id="41712-184">In the Package Manager Console, run the command **add-migration initial**.</span></span>

   5. <span data-ttu-id="41712-185">Uruchom polecenie **Update-Database**.</span><span class="sxs-lookup"><span data-stu-id="41712-185">Run the command **update-database**.</span></span>

      <span data-ttu-id="41712-186">Jeśli podczas uruchamiania tego polecenia zostanie wyświetlony wyjątek, wartości `StudentID` i `CourseID` mogą różnić się od wartości metody `Seed`.</span><span class="sxs-lookup"><span data-stu-id="41712-186">If you receive an exception when running this command, the `StudentID` and `CourseID` values might be different from the `Seed` method values.</span></span> <span data-ttu-id="41712-187">Otwórz te tabele bazy danych i Znajdź istniejące wartości dla `StudentID` i `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="41712-187">Open those database tables and find existing values for `StudentID` and `CourseID`.</span></span> <span data-ttu-id="41712-188">Dodaj te wartości do kodu umożliwiającego wypełnianie tabeli `Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="41712-188">Add those values to the code for seeding the `Enrollments` table.</span></span>

## <a name="add-a-gridview-control"></a><span data-ttu-id="41712-189">Dodaj kontrolkę GridView</span><span class="sxs-lookup"><span data-stu-id="41712-189">Add a GridView control</span></span>

<span data-ttu-id="41712-190">Przy użyciu wypełnionych danych bazy danych możesz teraz przystąpić do pobierania tych danych i wyświetlania ich.</span><span class="sxs-lookup"><span data-stu-id="41712-190">With populated database data, you're now ready to retrieve that data and display it.</span></span> 

1. <span data-ttu-id="41712-191">Otwórz uczniów. aspx.</span><span class="sxs-lookup"><span data-stu-id="41712-191">Open Students.aspx.</span></span>

2. <span data-ttu-id="41712-192">Znajdź `MainContent` symbol zastępczy.</span><span class="sxs-lookup"><span data-stu-id="41712-192">Locate the `MainContent` placeholder.</span></span> <span data-ttu-id="41712-193">W tym symbolu zastępczym Dodaj kontrolkę **GridView** , która zawiera ten kod.</span><span class="sxs-lookup"><span data-stu-id="41712-193">Within that placeholder, add a **GridView** control that includes this code.</span></span>

   [!code-aspx-csharp[Main](retrieving-data/samples/sample6.aspx)]

   <span data-ttu-id="41712-194">Kwestie do uwagi:</span><span class="sxs-lookup"><span data-stu-id="41712-194">Things to note:</span></span>
   * <span data-ttu-id="41712-195">Zwróć uwagę na wartość ustawioną dla właściwości `SelectMethod` w elemencie GridView.</span><span class="sxs-lookup"><span data-stu-id="41712-195">Notice the value set for the `SelectMethod` property in the GridView element.</span></span> <span data-ttu-id="41712-196">Ta wartość określa metodę używaną do pobierania danych GridView, które tworzysz w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="41712-196">This value specifies the method used to retrieve GridView data, which you create in the next step.</span></span> 
   
   * <span data-ttu-id="41712-197">Właściwość `ItemType` jest ustawiona na utworzoną wcześniej klasy `Student`.</span><span class="sxs-lookup"><span data-stu-id="41712-197">The `ItemType` property is set to the `Student` class created earlier.</span></span> <span data-ttu-id="41712-198">To ustawienie umożliwia odwołanie do właściwości klasy w znaczniku.</span><span class="sxs-lookup"><span data-stu-id="41712-198">This setting allows you to reference class properties in the markup.</span></span> <span data-ttu-id="41712-199">Na przykład Klasa `Student` ma kolekcję o nazwie `Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="41712-199">For example, the `Student` class has a collection named `Enrollments`.</span></span> <span data-ttu-id="41712-200">Możesz użyć `Item.Enrollments` do pobrania tej kolekcji, a następnie użyć [składni LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) , aby pobrać sumę kredytów zarejestrowanej przez studenta.</span><span class="sxs-lookup"><span data-stu-id="41712-200">You can use `Item.Enrollments` to retrieve that collection and then use [LINQ syntax](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) to retrieve each student's enrolled credits sum.</span></span>
   
3. <span data-ttu-id="41712-201">Zapisz uczniów. aspx.</span><span class="sxs-lookup"><span data-stu-id="41712-201">Save Students.aspx.</span></span>

## <a name="add-code-to-retrieve-data"></a><span data-ttu-id="41712-202">Dodawanie kodu w celu pobierania danych</span><span class="sxs-lookup"><span data-stu-id="41712-202">Add code to retrieve data</span></span>

   <span data-ttu-id="41712-203">W pliku student. aspx z kodem należy dodać metodę określoną dla wartości `SelectMethod`.</span><span class="sxs-lookup"><span data-stu-id="41712-203">In the Students.aspx code-behind file, add the method specified for the `SelectMethod` value.</span></span> 
   
   1. <span data-ttu-id="41712-204">Otwórz Students.aspx.cs.</span><span class="sxs-lookup"><span data-stu-id="41712-204">Open Students.aspx.cs.</span></span>
   
   2. <span data-ttu-id="41712-205">Dodaj `using` instrukcje dla przestrzeni nazw `ContosoUniversityModelBinding. Models` i `System.Data.Entity`.</span><span class="sxs-lookup"><span data-stu-id="41712-205">Add `using` statements for the `ContosoUniversityModelBinding. Models` and `System.Data.Entity` namespaces.</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample7.cs)]

   3. <span data-ttu-id="41712-206">Dodaj metodę określoną dla `SelectMethod`:</span><span class="sxs-lookup"><span data-stu-id="41712-206">Add the method you specified for `SelectMethod`:</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample8.cs)]

      <span data-ttu-id="41712-207">Klauzula `Include` podnosi wydajność zapytań, ale nie jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="41712-207">The `Include` clause improves query performance but isn't required.</span></span> <span data-ttu-id="41712-208">Bez klauzuli `Include` dane są pobierane przy użyciu [*ładowania opóźnionego*](https://en.wikipedia.org/wiki/Lazy_loading), które polega na wysyłaniu oddzielnego zapytania do bazy danych za każdym razem, gdy są pobierane powiązane dane.</span><span class="sxs-lookup"><span data-stu-id="41712-208">Without the `Include` clause, the data is retrieved using [*lazy loading*](https://en.wikipedia.org/wiki/Lazy_loading), which involves sending a separate query to the database each time related data is retrieved.</span></span> <span data-ttu-id="41712-209">Z klauzulą `Include` dane są pobierane przy użyciu *ładowania eager*, co oznacza, że pojedyncze zapytanie bazy danych pobiera wszystkie powiązane dane.</span><span class="sxs-lookup"><span data-stu-id="41712-209">With the `Include` clause, data is retrieved using *eager loading*, which means a single database query retrieves all related data.</span></span> <span data-ttu-id="41712-210">Jeśli powiązane dane nie są używane, ładowanie eager jest mniej wydajne, ponieważ jest pobieranych więcej danych.</span><span class="sxs-lookup"><span data-stu-id="41712-210">If related data isn't used, eager loading is less efficient because more data is retrieved.</span></span> <span data-ttu-id="41712-211">Jednak w takim przypadku ładowanie eager zapewnia najlepszą wydajność, ponieważ dla każdego rekordu są wyświetlane powiązane dane.</span><span class="sxs-lookup"><span data-stu-id="41712-211">However, in this case, eager loading gives you the best performance because the related data is displayed for each record.</span></span>

      <span data-ttu-id="41712-212">Aby uzyskać więcej informacji na temat zagadnień dotyczących wydajności podczas ładowania powiązanych danych, zobacz sekcję **opóźnione, Eager i jawne ładowanie powiązanych danych** w sekcji [odczytywanie powiązanych danych za pomocą Entity Framework w artykule ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) .</span><span class="sxs-lookup"><span data-stu-id="41712-212">For more information about performance considerations when loading related data, see the **Lazy, Eager, and Explicit Loading of Related Data** section in the [Reading Related Data with the Entity Framework in an ASP.NET MVC Application](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) article.</span></span>

      <span data-ttu-id="41712-213">Domyślnie dane są sortowane według wartości właściwości oznaczonej jako klucz.</span><span class="sxs-lookup"><span data-stu-id="41712-213">By default, the data is sorted by the values of the property marked as the key.</span></span> <span data-ttu-id="41712-214">Można dodać klauzulę `OrderBy`, aby określić inną wartość sortowania.</span><span class="sxs-lookup"><span data-stu-id="41712-214">You can add an `OrderBy` clause to specify a different sort value.</span></span> <span data-ttu-id="41712-215">W tym przykładzie domyślna właściwość `StudentID` jest używana do sortowania.</span><span class="sxs-lookup"><span data-stu-id="41712-215">In this example, the default `StudentID` property is used for sorting.</span></span> <span data-ttu-id="41712-216">W artykule [sortowanie, stronicowanie i filtrowanie danych](sorting-paging-and-filtering-data.md) użytkownik ma możliwość wybrania kolumny do sortowania.</span><span class="sxs-lookup"><span data-stu-id="41712-216">In the [Sorting, Paging, and Filtering Data](sorting-paging-and-filtering-data.md) article, the user is enabled to select a column for sorting.</span></span>
 
   4. <span data-ttu-id="41712-217">Zapisz Students.aspx.cs.</span><span class="sxs-lookup"><span data-stu-id="41712-217">Save Students.aspx.cs.</span></span>

## <a name="run-your-application"></a><span data-ttu-id="41712-218">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="41712-218">Run your application</span></span> 

<span data-ttu-id="41712-219">Uruchom aplikację sieci Web (**F5**) i przejdź do strony **uczniów** , która wyświetla następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="41712-219">Run your web application (**F5**) and navigate to the **Students** page, which displays the following:</span></span>

   ![Pokaż dane](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a><span data-ttu-id="41712-221">Automatyczne generowanie metod powiązania modelu</span><span class="sxs-lookup"><span data-stu-id="41712-221">Automatic generation of model binding methods</span></span>

<span data-ttu-id="41712-222">Podczas pracy z tą serią samouczków możesz po prostu skopiować kod z samouczka do projektu.</span><span class="sxs-lookup"><span data-stu-id="41712-222">When working through this tutorial series, you can simply copy the code from the tutorial to your project.</span></span> <span data-ttu-id="41712-223">Jednak jedną wadą tego podejścia jest to, że użytkownik może nie być świadomy funkcji dostarczonej przez program Visual Studio w celu automatycznego generowania kodu dla metod powiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="41712-223">However, one disadvantage of this approach is that you may not become aware of the feature provided by Visual Studio to automatically generate code for model binding methods.</span></span> <span data-ttu-id="41712-224">Podczas pracy nad własnymi projektami automatyczne generowanie kodu może zaoszczędzić czas i pomóc w uzyskaniu tego, jak zaimplementować operację.</span><span class="sxs-lookup"><span data-stu-id="41712-224">When working on your own projects, automatic code generation can save you time and help you gain a sense of how to implement an operation.</span></span> <span data-ttu-id="41712-225">W tej sekcji opisano funkcję automatycznego generowania kodu.</span><span class="sxs-lookup"><span data-stu-id="41712-225">This section describes the automatic code generation feature.</span></span> <span data-ttu-id="41712-226">Ta sekcja jest tylko informacyjna i nie zawiera kodu, który jest wymagany do zaimplementowania w projekcie.</span><span class="sxs-lookup"><span data-stu-id="41712-226">This section is only informational and does not contain any code you need to implement in your project.</span></span> 

<span data-ttu-id="41712-227">Podczas ustawiania wartości właściwości `SelectMethod`, `UpdateMethod`, `InsertMethod`lub `DeleteMethod` w kodzie znaczników można wybrać opcję **Utwórz nową metodę** .</span><span class="sxs-lookup"><span data-stu-id="41712-227">When setting a value for the `SelectMethod`, `UpdateMethod`, `InsertMethod`, or `DeleteMethod` properties in the markup code, you can select the **Create New Method** option.</span></span>

![Tworzenie metody](retrieving-data/_static/image18.png)

<span data-ttu-id="41712-229">Program Visual Studio nie tylko tworzy metodę w kodzie związanym z prawidłowym podpisem, ale również generuje kod implementacji do wykonania operacji.</span><span class="sxs-lookup"><span data-stu-id="41712-229">Visual Studio not only creates a method in the code-behind with the proper signature, but also generates implementation code to perform the operation.</span></span> <span data-ttu-id="41712-230">Jeśli najpierw ustawisz właściwość `ItemType` przed użyciem funkcji automatycznego generowania kodu, wygenerowany kod używa tego typu dla operacji.</span><span class="sxs-lookup"><span data-stu-id="41712-230">If you first set the `ItemType` property before using the automatic code generation feature, the generated code uses that type for the operations.</span></span> <span data-ttu-id="41712-231">Na przykład podczas ustawiania właściwości `UpdateMethod` następujący kod jest generowany automatycznie:</span><span class="sxs-lookup"><span data-stu-id="41712-231">For example, when setting the `UpdateMethod` property, the following code is automatically generated:</span></span>

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

<span data-ttu-id="41712-232">Ponownie ten kod nie musi być dodany do projektu.</span><span class="sxs-lookup"><span data-stu-id="41712-232">Again, this code doesn't need to be added to your project.</span></span> <span data-ttu-id="41712-233">W następnym samouczku zostaną zaimplementowane metody aktualizowania, usuwania i dodawania nowych danych.</span><span class="sxs-lookup"><span data-stu-id="41712-233">In the next tutorial, you'll implement methods for updating, deleting, and adding new data.</span></span>

## <a name="summary"></a><span data-ttu-id="41712-234">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="41712-234">Summary</span></span>

<span data-ttu-id="41712-235">W tym samouczku utworzono klasy modelu danych i Wygenerowano bazę danych z tych klas.</span><span class="sxs-lookup"><span data-stu-id="41712-235">In this tutorial, you created data model classes and generated a database from those classes.</span></span> <span data-ttu-id="41712-236">Tabele bazy danych zostały wypełnione danymi testowymi.</span><span class="sxs-lookup"><span data-stu-id="41712-236">You filled the database tables with test data.</span></span> <span data-ttu-id="41712-237">Użyto powiązania modelu do pobierania danych z bazy danych, a następnie wyświetlania danych w widoku GridView.</span><span class="sxs-lookup"><span data-stu-id="41712-237">You used model binding to retrieve data from the database, and then displayed the data in a GridView.</span></span>

<span data-ttu-id="41712-238">W następnym [samouczku](updating-deleting-and-creating-data.md) w tej serii będziesz włączać aktualizowanie, usuwanie i tworzenie danych.</span><span class="sxs-lookup"><span data-stu-id="41712-238">In the next [tutorial](updating-deleting-and-creating-data.md) in this series, you'll enable updating, deleting, and creating data.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="41712-239">Next</span><span class="sxs-lookup"><span data-stu-id="41712-239">Next</span></span>](updating-deleting-and-creating-data.md)
