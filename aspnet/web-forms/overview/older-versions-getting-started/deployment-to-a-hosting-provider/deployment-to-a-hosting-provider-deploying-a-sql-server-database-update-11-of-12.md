---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: 'Wdrażanie SQL Server Compact aplikacji sieci Web ASP.NET za pomocą programu Visual Studio lub Visual Web Developer: wdrażanie aktualizacji bazy danych SQL Server Database-11 z 12 | Microsoft Docs'
author: tdykstra
description: W tej serii samouczków pokazano, jak wdrożyć (opublikować) projekt aplikacji sieci Web ASP.NET, który zawiera bazę danych SQL Server Compact przy użyciu stu Visual...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: 0894c0ac24737e66b6960ef3d48aa17f78c6aa1d
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74621081"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a><span data-ttu-id="b0731-103">Wdrażanie SQL Server Compact aplikacji sieci Web ASP.NET za pomocą programu Visual Studio lub Visual Web Developer: wdrażanie aktualizacji SQL Server Database — 11 z 12</span><span class="sxs-lookup"><span data-stu-id="b0731-103">Deploying an ASP.NET Web Application with SQL Server Compact using Visual Studio or Visual Web Developer: Deploying a SQL Server Database Update - 11 of 12</span></span>

<span data-ttu-id="b0731-104">Autor [Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="b0731-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="b0731-105">Pobierz projekt początkowy</span><span class="sxs-lookup"><span data-stu-id="b0731-105">Download Starter Project</span></span>](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> <span data-ttu-id="b0731-106">W tej serii samouczków pokazano, jak wdrożyć (opublikować) projekt aplikacji sieci Web ASP.NET, który zawiera bazę danych SQL Server Compact przy użyciu programu Visual Studio 2012 RC lub Visual Studio Express 2012 RC dla sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b0731-106">This series of tutorials shows you how to deploy (publish) an ASP.NET web application project that includes a SQL Server Compact database by using Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web.</span></span> <span data-ttu-id="b0731-107">Możesz również użyć programu Visual Studio 2010, jeśli zostanie zainstalowana aktualizacja publikacji w sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b0731-107">You can also use Visual Studio 2010 if you install the Web Publish Update.</span></span> <span data-ttu-id="b0731-108">Aby zapoznać się z wprowadzeniem do serii, zobacz [pierwszy samouczek w serii](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="b0731-108">For an introduction to the series, see [the first tutorial in the series](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span></span>
> 
> <span data-ttu-id="b0731-109">Aby zapoznać się z samouczkiem zawierającym funkcje wdrażania wprowadzone po wydaniu wersji RC programu Visual Studio 2012, pokazano, jak wdrożyć wersje SQL Server inne niż SQL Server Compact i jak wdrożyć je w witrynach sieci Web systemu Windows Azure, zobacz [ASP.NET Web Deployment za pomocą programu Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b0731-109">For a tutorial that shows deployment features introduced after the RC release of Visual Studio 2012, shows how to deploy SQL Server editions other than SQL Server Compact, and shows how to deploy to Windows Azure Web Sites, see [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span></span>

## <a name="overview"></a><span data-ttu-id="b0731-110">Omówienie</span><span class="sxs-lookup"><span data-stu-id="b0731-110">Overview</span></span>

<span data-ttu-id="b0731-111">W tym samouczku pokazano, jak wdrożyć aktualizację bazy danych w pełnej SQL Server bazie danych.</span><span class="sxs-lookup"><span data-stu-id="b0731-111">This tutorial shows you how to deploy a database update to a full SQL Server database.</span></span> <span data-ttu-id="b0731-112">Ponieważ Migracje Code First wykonuje wszystkie czynności związane z aktualizacją bazy danych, proces jest niemal identyczny z tym, co zostało zrobione SQL Server Compact w samouczku [wdrażanie aktualizacji bazy danych](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) .</span><span class="sxs-lookup"><span data-stu-id="b0731-112">Because Code First Migrations does all the work of updating the database, the process is almost identical to what you did for SQL Server Compact in the [Deploying a Database Update](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) tutorial.</span></span>

<span data-ttu-id="b0731-113">Przypomnienie: Jeśli zostanie wyświetlony komunikat o błędzie lub coś nie działa, gdy przejdziesz do samouczka, pamiętaj o sprawdzeniu [strony rozwiązywania problemów](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="b0731-113">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span></span>

## <a name="adding-a-new-column-to-a-table"></a><span data-ttu-id="b0731-114">Dodawanie nowej kolumny do tabeli</span><span class="sxs-lookup"><span data-stu-id="b0731-114">Adding a New Column to a Table</span></span>

<span data-ttu-id="b0731-115">W tej części samouczka wprowadzisz zmiany w bazie danych i odpowiednie zmiany w kodzie, a następnie przetestuj je w programie Visual Studio w celu wdrożenia ich w środowisku testowym i produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="b0731-115">In this section of the tutorial you'll make a database change and corresponding code changes, then test them in Visual Studio in preparation for deploying them to the test and production environments.</span></span> <span data-ttu-id="b0731-116">Zmiana obejmuje Dodawanie kolumny `OfficeHours` do jednostki `Instructor` i wyświetlanie nowych informacji na stronie sieci Web **instruktorów** .</span><span class="sxs-lookup"><span data-stu-id="b0731-116">The change involves adding an `OfficeHours` column to the `Instructor` entity and displaying the new information in the **Instructors** web page.</span></span>

<span data-ttu-id="b0731-117">W projekcie ContosoUniversity. DAL Otwórz *Instructor.cs* i Dodaj następującą właściwość między `HireDate` i `Courses` właściwości:</span><span class="sxs-lookup"><span data-stu-id="b0731-117">In the ContosoUniversity.DAL project, open *Instructor.cs* and add the following property between the `HireDate` and `Courses` properties:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

<span data-ttu-id="b0731-118">Zaktualizuj klasę inicjatora, aby umożliwić jej odsiewanie nowej kolumny przy użyciu danych testowych.</span><span class="sxs-lookup"><span data-stu-id="b0731-118">Update the initializer class so that it seeds the new column with test data.</span></span> <span data-ttu-id="b0731-119">Otwórz *Migrations\Configuration.cs* i Zastąp blok kodu, który rozpoczyna się `var instructors = new List<Instructor>` z następującym blokiem kodu, który zawiera nową kolumnę:</span><span class="sxs-lookup"><span data-stu-id="b0731-119">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes the new column:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

<span data-ttu-id="b0731-120">W projekcie ContosoUniversity Otwórz *instruktors. aspx* i Dodaj nowe pole szablonu dla godzin pracy tuż przed tagiem zamykającym `</Columns>` w pierwszej kontrolce `GridView`:</span><span class="sxs-lookup"><span data-stu-id="b0731-120">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field for office hours just before the closing `</Columns>` tag in the first `GridView` control:</span></span>

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

<span data-ttu-id="b0731-121">Skompiluj rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="b0731-121">Build the solution.</span></span>

<span data-ttu-id="b0731-122">Otwórz okno **konsoli Menedżera pakietów** i wybierz pozycję CONTOSOUNIVERSITY. dal jako **Projekt domyślny**.</span><span class="sxs-lookup"><span data-stu-id="b0731-122">Open the **Package Manager Console** window, and select ContosoUniversity.DAL as the **Default project**.</span></span>

<span data-ttu-id="b0731-123">Wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="b0731-123">Enter the following commands:</span></span>

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

<span data-ttu-id="b0731-124">Uruchom aplikację i wybierz stronę **Instruktorzy** .</span><span class="sxs-lookup"><span data-stu-id="b0731-124">Run the application and select the **Instructors** page.</span></span> <span data-ttu-id="b0731-125">Ładowanie strony trwa nieco dłużej niż zwykle, ponieważ usługa Entity Framework ponownie tworzy bazę danych i wystawia ją z danymi testowymi.</span><span class="sxs-lookup"><span data-stu-id="b0731-125">The page takes a little longer than usual to load, because the Entity Framework re-creates the database and seeds it with test data.</span></span>

<span data-ttu-id="b0731-126">[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b0731-126">[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)</span></span>

## <a name="deploying-the-database-update-to-the-test-environment"></a><span data-ttu-id="b0731-127">Wdrażanie aktualizacji bazy danych w środowisku testowym</span><span class="sxs-lookup"><span data-stu-id="b0731-127">Deploying the Database Update to the Test Environment</span></span>

<span data-ttu-id="b0731-128">W przypadku korzystania z Migracje Code First Metoda wdrażania bazy danych zmiana na SQL Server jest taka sama jak w przypadku SQL Server Compact.</span><span class="sxs-lookup"><span data-stu-id="b0731-128">When you use Code First Migrations, the method for deploying a database change to SQL Server is the same as for SQL Server Compact.</span></span> <span data-ttu-id="b0731-129">Należy jednak zmienić profil publikowania testowego, ponieważ nadal jest on skonfigurowany do migracji z SQL Server Compact do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b0731-129">However, you have to change the Test publish profile because it is still set up to migrate from SQL Server Compact to SQL Server.</span></span>

<span data-ttu-id="b0731-130">Pierwszym krokiem jest usunięcie przekształceń parametrów połączenia utworzonych w poprzednim samouczku.</span><span class="sxs-lookup"><span data-stu-id="b0731-130">The first step is to remove the connection string transformations that you created in the previous tutorial.</span></span> <span data-ttu-id="b0731-131">Te nie są już potrzebne, ponieważ w profilu publikacji określisz przekształcenia parametrów połączenia, tak jak wcześniej przed skonfigurowaniem na karcie **pakiet/publikowanie danych SQL** na potrzeby migracji do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b0731-131">These are no longer needed because you'll specify connection string transformations in the publish profile, as you did before you configured the **Package/Publish SQL** tab for migration to SQL Server.</span></span>

<span data-ttu-id="b0731-132">Otwórz plik *Web. test. config* i usuń element `connectionStrings`.</span><span class="sxs-lookup"><span data-stu-id="b0731-132">Open the *Web.Test.config* file and remove the `connectionStrings` element.</span></span> <span data-ttu-id="b0731-133">Jedyna pozostała transformacja w pliku *Web. test. config* dotyczy wartości `Environment` w elemencie `appSettings`.</span><span class="sxs-lookup"><span data-stu-id="b0731-133">The only remaining transformation in the *Web.Test.config* file is for the `Environment` value in the `appSettings` element.</span></span>

<span data-ttu-id="b0731-134">Teraz można zaktualizować profil publikowania i opublikować go w środowisku testowym.</span><span class="sxs-lookup"><span data-stu-id="b0731-134">Now you can update the publish profile and publish to the test environment.</span></span>

<span data-ttu-id="b0731-135">Otwórz kreatora **publikacji w sieci Web** , a następnie przejdź do karty **profil** .</span><span class="sxs-lookup"><span data-stu-id="b0731-135">Open the **Publish Web** wizard, and then switch to the **Profile** tab.</span></span>

<span data-ttu-id="b0731-136">Wybierz profil publikowania **testowego** .</span><span class="sxs-lookup"><span data-stu-id="b0731-136">Select the **Test** publish profile.</span></span>

<span data-ttu-id="b0731-137">Wybierz kartę **Ustawienia** .</span><span class="sxs-lookup"><span data-stu-id="b0731-137">Select the **Settings** tab.</span></span>

<span data-ttu-id="b0731-138">Kliknij pozycję **Włącz nowe ulepszenia publikowania bazy danych**.</span><span class="sxs-lookup"><span data-stu-id="b0731-138">Click **enable the new database publishing improvements**.</span></span>

<span data-ttu-id="b0731-139">W polu ciąg połączenia dla **SchoolContext**wprowadź tę samą wartość, która była używana w pliku transformacji *Web. test. config* w poprzednim samouczku:</span><span class="sxs-lookup"><span data-stu-id="b0731-139">In the connection string box for **SchoolContext**, enter the same value that you used in the *Web.Test.config* transformation file in the previous tutorial:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

<span data-ttu-id="b0731-140">Wybierz pozycję **wykonaj migracje Code First (uruchamiaj przy uruchamianiu aplikacji)** .</span><span class="sxs-lookup"><span data-stu-id="b0731-140">Select **Execute Code First Migrations (runs on application start)**.</span></span> <span data-ttu-id="b0731-141">(W używanej wersji programu Visual Studio, pole wyboru może mieć etykietę **zastosuj migracje Code First**.)</span><span class="sxs-lookup"><span data-stu-id="b0731-141">(In your version of Visual Studio, the check box might be labeled **Apply Code First Migrations**.)</span></span>

<span data-ttu-id="b0731-142">W polu ciąg połączenia dla **DefaultConnection**wprowadź tę samą wartość, która była używana w pliku transformacji *Web. test. config* w poprzednim samouczku:</span><span class="sxs-lookup"><span data-stu-id="b0731-142">In the connection string box for **DefaultConnection**, enter the same value that you used in the *Web.Test.config* transformation file in the previous tutorial:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

<span data-ttu-id="b0731-143">Pozostaw wyczyszczone **aktualizacje bazy danych** .</span><span class="sxs-lookup"><span data-stu-id="b0731-143">Leave **Update database** cleared.</span></span>

<span data-ttu-id="b0731-144">Kliknij przycisk **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="b0731-144">Click **Publish**.</span></span>

<span data-ttu-id="b0731-145">Program Visual Studio wdraża zmiany kodu w środowisku testowym i otwiera przeglądarkę na stronie głównej firmy Contoso University.</span><span class="sxs-lookup"><span data-stu-id="b0731-145">Visual Studio deploys the code changes to the test environment and opens the browser to the Contoso University home page.</span></span>

<span data-ttu-id="b0731-146">Wybierz stronę instruktorów.</span><span class="sxs-lookup"><span data-stu-id="b0731-146">Select the Instructors page.</span></span>

<span data-ttu-id="b0731-147">Gdy aplikacja uruchamia Tę stronę, próbuje uzyskać dostęp do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b0731-147">When the application runs this page, it tries to access the database.</span></span> <span data-ttu-id="b0731-148">Migracje Code First sprawdza, czy baza danych jest aktualna, i stwierdza, że nie zastosowano jeszcze najnowszej migracji.</span><span class="sxs-lookup"><span data-stu-id="b0731-148">Code First Migrations checks if the database is current, and finds that the latest migration has not been applied yet.</span></span> <span data-ttu-id="b0731-149">Migracje Code First stosuje najnowszą migrację, uruchamia metodę `Seed`, a następnie strona działa normalnie.</span><span class="sxs-lookup"><span data-stu-id="b0731-149">Code First Migrations applies the latest migration, runs the `Seed` method, and then the page runs normally.</span></span> <span data-ttu-id="b0731-150">Zostanie wyświetlona kolumna nowe godziny pracy z danymi z rozrzutu.</span><span class="sxs-lookup"><span data-stu-id="b0731-150">You see the new Office Hours column with the seeded data.</span></span>

<span data-ttu-id="b0731-151">[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="b0731-151">[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)</span></span>

## <a name="deploying-the-database-update-to-the-production-environment"></a><span data-ttu-id="b0731-152">Wdrażanie aktualizacji bazy danych w środowisku produkcyjnym</span><span class="sxs-lookup"><span data-stu-id="b0731-152">Deploying the Database Update to the Production Environment</span></span>

<span data-ttu-id="b0731-153">Należy również zmienić profil publikowania dla środowiska produkcyjnego.</span><span class="sxs-lookup"><span data-stu-id="b0731-153">You have to change the publish profile for the production environment also.</span></span> <span data-ttu-id="b0731-154">W takim przypadku należy usunąć istniejący profil i utworzyć nowy, importując zaktualizowany plik. publishsettings.</span><span class="sxs-lookup"><span data-stu-id="b0731-154">In this case you'll remove the existing profile and create a new one by importing an updated .publishsettings file.</span></span> <span data-ttu-id="b0731-155">Zaktualizowany plik będzie zawierać parametry połączenia dla bazy danych SQL Server w Cytanium.</span><span class="sxs-lookup"><span data-stu-id="b0731-155">The updated file will include the connection string for the SQL Server database at Cytanium.</span></span>

<span data-ttu-id="b0731-156">Podczas wdrażania w środowisku testowym nie są już potrzebne transformacje parametrów połączenia w pliku transformacji *Web. Production. config* .</span><span class="sxs-lookup"><span data-stu-id="b0731-156">As you saw when you deployed to the test environment, you no longer need connection string transforms in the *Web.Production.config* transformation file.</span></span> <span data-ttu-id="b0731-157">Otwórz ten plik i Usuń element `connectionStrings`.</span><span class="sxs-lookup"><span data-stu-id="b0731-157">Open that file and remove the `connectionStrings` element.</span></span> <span data-ttu-id="b0731-158">Pozostałe przekształcenia dotyczą wartości `Environment` w elemencie `appSettings` i elementu `location`, który ogranicza dostęp do raportów o błędach ELMAH.</span><span class="sxs-lookup"><span data-stu-id="b0731-158">The remaining transformations are for the `Environment` value in the `appSettings` element and the `location` element that restricts access to Elmah error reports.</span></span>

<span data-ttu-id="b0731-159">Przed utworzeniem nowego profilu publikowania dla środowiska produkcyjnego Pobierz zaktualizowany plik. publishsettings w taki sam sposób jak wcześniej w samouczku [wdrażanie w środowisku produkcyjnym](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) .</span><span class="sxs-lookup"><span data-stu-id="b0731-159">Before you create a new publish profile for production, download an updated .publishsettings file the same way you did earlier in the [Deploying to the Production Environment](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial.</span></span> <span data-ttu-id="b0731-160">(W panelu sterowania Cytanium kliknij pozycję **witryny sieci Web**, a następnie kliknij witrynę internetową **contosouniversity.com** .</span><span class="sxs-lookup"><span data-stu-id="b0731-160">(In the Cytanium control panel, click **Web Sites**, and then click the **contosouniversity.com** website.</span></span> <span data-ttu-id="b0731-161">Wybierz kartę **Publikowanie w sieci Web** , a następnie kliknij pozycję **Pobierz profil publikowania dla tej witryny sieci Web**. Przyczyną tego problemu jest pobranie parametrów połączenia bazy danych w pliku. publishsettings.</span><span class="sxs-lookup"><span data-stu-id="b0731-161">Select the **Web Publishing** tab, and then click **Download Publishing Profile for this web site**.) The reason you are doing this is to pick up the database connection string in the .publishsettings file.</span></span> <span data-ttu-id="b0731-162">Parametry połączenia nie były dostępne przy pierwszym pobraniu pliku, ponieważ nadal używasz SQL Server Compact i nie utworzyć SQL Server bazę danych w Cytanium.</span><span class="sxs-lookup"><span data-stu-id="b0731-162">The connection string wasn't available the first time you downloaded the file, because you were still using SQL Server Compact and hadn't created the SQL Server database at Cytanium yet.</span></span>

<span data-ttu-id="b0731-163">Teraz można zaktualizować profil publikowania i opublikować go w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="b0731-163">Now you can update the publish profile and publish to the production environment.</span></span>

<span data-ttu-id="b0731-164">Otwórz kreatora **publikacji w sieci Web** , a następnie przejdź do karty **profil** .</span><span class="sxs-lookup"><span data-stu-id="b0731-164">Open the **Publish Web** wizard, and then switch to the **Profile** tab.</span></span>

<span data-ttu-id="b0731-165">Kliknij pozycję **Zarządzaj profilami**, a następnie Usuń profil produkcyjny.</span><span class="sxs-lookup"><span data-stu-id="b0731-165">Click **Manage Profiles**, and then delete the Production profile.</span></span>

<span data-ttu-id="b0731-166">Zamknij kreatora **publikacji w sieci Web** , aby zapisać tę zmianę.</span><span class="sxs-lookup"><span data-stu-id="b0731-166">Close the **Publish Web** wizard to save this change.</span></span>

<span data-ttu-id="b0731-167">Otwórz ponownie kreatora **publikacji sieci Web** , a następnie kliknij przycisk **Importuj**.</span><span class="sxs-lookup"><span data-stu-id="b0731-167">Open the **Publish Web** wizard again, and then click **Import**.</span></span>

<span data-ttu-id="b0731-168">Na karcie **połączenie** Zmień **docelowy adres URL** na odpowiednią wartość, jeśli używasz tymczasowego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="b0731-168">On the **Connection** tab, change **Destination URL** to the appropriate value if you are using a temporary URL.</span></span>

<span data-ttu-id="b0731-169">Kliknij przycisk **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="b0731-169">Click **Next**.</span></span>

<span data-ttu-id="b0731-170">Na karcie **Ustawienia** kliknij pozycję **Włącz nowe ulepszenia publikowania bazy danych**.</span><span class="sxs-lookup"><span data-stu-id="b0731-170">On the **Settings** tab, click **enable the new database publishing improvements**.</span></span>

<span data-ttu-id="b0731-171">Z listy rozwijanej parametry połączenia dla **SchoolContext**Wybierz parametry połączenia Cytanium.</span><span class="sxs-lookup"><span data-stu-id="b0731-171">In the connection string drop-down list for **SchoolContext**, select the Cytanium connection string.</span></span>

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

<span data-ttu-id="b0731-173">Wybierz pozycję **Wykonaj migracje Code First (działa przy uruchamianiu aplikacji)** .</span><span class="sxs-lookup"><span data-stu-id="b0731-173">Select **Execute Code First migrations (runs on application start)**.</span></span>

<span data-ttu-id="b0731-174">Z listy rozwijanej parametry połączenia dla **DefaultConnection**Wybierz parametry połączenia Cytanium.</span><span class="sxs-lookup"><span data-stu-id="b0731-174">In the connection string drop-down list for **DefaultConnection**, select the Cytanium connection string.</span></span>

<span data-ttu-id="b0731-175">Wybierz kartę **profil** , kliknij pozycję **Zarządzaj**profilami, a następnie zmień nazwę profilu z "contosouniversity.com Web Deploy" na "produkcja".</span><span class="sxs-lookup"><span data-stu-id="b0731-175">Select the **Profile** tab, click **Manage Profiles**, and rename the profile from "contosouniversity.com - Web Deploy" to "Production".</span></span>

<span data-ttu-id="b0731-176">Zamknij profil publikowania, aby zapisać zmiany, a następnie otwórz je ponownie.</span><span class="sxs-lookup"><span data-stu-id="b0731-176">Close the publish profile to save the change, then open it again.</span></span>

<span data-ttu-id="b0731-177">Kliknij przycisk **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="b0731-177">Click **Publish**.</span></span> <span data-ttu-id="b0731-178">(W przypadku rzeczywistej produkcyjnej witryny sieci Web należy skopiować *aplikację\_offline. htm* do środowiska produkcyjnego i umieścić ją w folderze projektu przed opublikowaniem, a następnie usunąć ją po zakończeniu wdrażania.)</span><span class="sxs-lookup"><span data-stu-id="b0731-178">(For a real production website, you would copy *app\_offline.htm* to production and put it in your project folder before publishing, then remove it when deployment is complete.)</span></span>

<span data-ttu-id="b0731-179">Program Visual Studio wdraża zmiany kodu w środowisku testowym i otwiera przeglądarkę na stronie głównej firmy Contoso University.</span><span class="sxs-lookup"><span data-stu-id="b0731-179">Visual Studio deploys the code changes to the test environment and opens the browser to the Contoso University home page.</span></span>

<span data-ttu-id="b0731-180">Wybierz stronę instruktorów.</span><span class="sxs-lookup"><span data-stu-id="b0731-180">Select the Instructors page.</span></span>

<span data-ttu-id="b0731-181">Migracje Code First aktualizuje bazę danych w taki sam sposób jak w środowisku testowym.</span><span class="sxs-lookup"><span data-stu-id="b0731-181">Code First Migrations updates the database the same way it did in the Test environment.</span></span> <span data-ttu-id="b0731-182">Zostanie wyświetlona kolumna nowe godziny pracy z danymi z rozrzutu.</span><span class="sxs-lookup"><span data-stu-id="b0731-182">You see the new Office Hours column with the seeded data.</span></span>

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

<span data-ttu-id="b0731-184">Pomyślnie wdrożono aktualizację aplikacji, która obejmowała zmianę bazy danych, przy użyciu bazy danych SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b0731-184">You have now successfully deployed an application update that included a database change, using a SQL Server database.</span></span>

## <a name="more-information"></a><span data-ttu-id="b0731-185">Więcej informacji</span><span class="sxs-lookup"><span data-stu-id="b0731-185">More Information</span></span>

<span data-ttu-id="b0731-186">Ta seria samouczków dotyczy wdrażania aplikacji sieci Web ASP.NET w ramach dostawcy hostingu innej firmy.</span><span class="sxs-lookup"><span data-stu-id="b0731-186">This completes this series of tutorials on deploying an ASP.NET web application to a third-party hosting provider.</span></span> <span data-ttu-id="b0731-187">Aby uzyskać więcej informacji na temat tematów omówionych w tych samouczkach, zobacz [mapę zawartości wdrożenia ASP.NET](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) w witrynie MSDN w sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b0731-187">For more information about any of the topics covered in these tutorials, see the [ASP.NET Deployment Content Map](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) on the MSDN web site.</span></span>

## <a name="acknowledgements"></a><span data-ttu-id="b0731-188">Podziękowania</span><span class="sxs-lookup"><span data-stu-id="b0731-188">Acknowledgements</span></span>

<span data-ttu-id="b0731-189">Chcę podziękowanie następujące osoby, które złożyły znaczące wkłady w zawartość tej serii samouczków:</span><span class="sxs-lookup"><span data-stu-id="b0731-189">I would like to thank the following people who made significant contributions to the content of this tutorial series:</span></span>

- [<span data-ttu-id="b0731-190">Alberto Poblacion, MVP &amp; MCT, Hiszpania</span><span class="sxs-lookup"><span data-stu-id="b0731-190">Alberto Poblacion, MVP &amp; MCT, Spain</span></span>](https://mvp.support.microsoft.com/profile/Alberto)
- <span data-ttu-id="b0731-191">Jarod Ferguson, SPECJALISTa ds. projektowania platformy danych, Stany Zjednoczone</span><span class="sxs-lookup"><span data-stu-id="b0731-191">Jarod Ferguson, Data Platform Development MVP, United States</span></span>
- <span data-ttu-id="b0731-192">Harsh Mittal, Microsoft</span><span class="sxs-lookup"><span data-stu-id="b0731-192">Harsh Mittal, Microsoft</span></span>
- [<span data-ttu-id="b0731-193">Kristina Olson, Microsoft</span><span class="sxs-lookup"><span data-stu-id="b0731-193">Kristina Olson, Microsoft</span></span>](https://blogs.iis.net/krolson/default.aspx)
- [<span data-ttu-id="b0731-194">Jan Pope, Microsoft</span><span class="sxs-lookup"><span data-stu-id="b0731-194">Mike Pope, Microsoft</span></span>](http://www.mikepope.com/blog/DisplayBlog.aspx)
- <span data-ttu-id="b0731-195">Mohit Srivastava, Microsoft</span><span class="sxs-lookup"><span data-stu-id="b0731-195">Mohit Srivastava, Microsoft</span></span>
- [<span data-ttu-id="b0731-196">Raffaele Rialdi, Włochy</span><span class="sxs-lookup"><span data-stu-id="b0731-196">Raffaele Rialdi, Italy</span></span>](http://www.iamraf.net/)
- [<span data-ttu-id="b0731-197">Rick Anderson, Microsoft</span><span class="sxs-lookup"><span data-stu-id="b0731-197">Rick Anderson, Microsoft</span></span>](https://blogs.msdn.com/b/rickandy/)
- <span data-ttu-id="b0731-198">[Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [@sayedihashimi](http://twitter.com/sayedihashimi))</span><span class="sxs-lookup"><span data-stu-id="b0731-198">[Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [@sayedihashimi](http://twitter.com/sayedihashimi))</span></span>
- <span data-ttu-id="b0731-199">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](http://twitter.com/shanselman))</span><span class="sxs-lookup"><span data-stu-id="b0731-199">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](http://twitter.com/shanselman))</span></span>
- <span data-ttu-id="b0731-200">[Scott myśliwy, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [@coolcsh](http://twitter.com/coolcsh))</span><span class="sxs-lookup"><span data-stu-id="b0731-200">[Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [@coolcsh](http://twitter.com/coolcsh))</span></span>
- [<span data-ttu-id="b0731-201">Srđan Božović, Serbia</span><span class="sxs-lookup"><span data-stu-id="b0731-201">Srđan Božović, Serbia</span></span>](http://msforge.net/blogs/zmajcek/)
- <span data-ttu-id="b0731-202">[Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [@vishalrjoshi](http://twitter.com/vishalrjoshi))</span><span class="sxs-lookup"><span data-stu-id="b0731-202">[Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [@vishalrjoshi](http://twitter.com/vishalrjoshi))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b0731-203">[Poprzednie](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
> [dalej](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)</span><span class="sxs-lookup"><span data-stu-id="b0731-203">[Previous](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
[Next](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)</span></span>
