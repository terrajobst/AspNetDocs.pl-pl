---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: 'ASP.NET wdrażanie w sieci Web przy użyciu programu Visual Studio: wdrażanie aktualizacji bazy danych | Microsoft Docs'
author: tdykstra
description: W tej serii samouczków pokazano, jak wdrożyć (opublikować) aplikację sieci Web ASP.NET w celu Azure App Service Web Apps lub do dostawcy hostingu innej firmy przez usin...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 805eb84c24764cf921291f89054435601dbac48e
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74636831"
---
# <a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a><span data-ttu-id="af0aa-103">ASP.NET wdrażanie w sieci Web przy użyciu programu Visual Studio: wdrażanie aktualizacji bazy danych</span><span class="sxs-lookup"><span data-stu-id="af0aa-103">ASP.NET Web Deployment using Visual Studio: Deploying a Database Update</span></span>

<span data-ttu-id="af0aa-104">Autor [Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="af0aa-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="af0aa-105">Pobierz projekt początkowy</span><span class="sxs-lookup"><span data-stu-id="af0aa-105">Download Starter Project</span></span>](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="af0aa-106">W tej serii samouczków pokazano, jak wdrożyć (opublikować) aplikację sieci Web ASP.NET w celu Azure App Service Web Apps lub do dostawcy hostingu innej firmy przy użyciu programu Visual Studio 2012 lub Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="af0aa-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="af0aa-107">Aby uzyskać informacje o serii, zobacz [pierwszy samouczek w serii](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="af0aa-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>

## <a name="overview"></a><span data-ttu-id="af0aa-108">Omówienie</span><span class="sxs-lookup"><span data-stu-id="af0aa-108">Overview</span></span>

<span data-ttu-id="af0aa-109">W tym samouczku wprowadzisz zmiany w bazie danych i powiązane zmiany kodu, testujesz zmiany w programie Visual Studio, a następnie wdrażasz aktualizację w środowiskach testowych, przejściowych i produkcyjnych.</span><span class="sxs-lookup"><span data-stu-id="af0aa-109">In this tutorial, you make a database change and related code changes, test the changes in Visual Studio, then deploy the update to the test, staging, and production environments.</span></span>

<span data-ttu-id="af0aa-110">W tym samouczku pokazano, jak zaktualizować bazę danych zarządzaną przez program Migracje Code First, a później pokazano, jak zaktualizować bazę danych za pomocą dostawcy dbDacFx.</span><span class="sxs-lookup"><span data-stu-id="af0aa-110">The tutorial first shows how to update a database that is managed by Code First Migrations, and then later it shows how to update a database by using the dbDacFx provider.</span></span>

<span data-ttu-id="af0aa-111">Przypomnienie: Jeśli zostanie wyświetlony komunikat o błędzie lub coś nie działa, gdy przejdziesz do samouczka, pamiętaj o sprawdzeniu [strony rozwiązywania problemów](troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="af0aa-111">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a><span data-ttu-id="af0aa-112">Wdrażanie aktualizacji bazy danych za pomocą Migracje Code First</span><span class="sxs-lookup"><span data-stu-id="af0aa-112">Deploy a database update by using Code First Migrations</span></span>

<span data-ttu-id="af0aa-113">W tej sekcji dodasz kolumnę Data urodzenia do `Person` klasy bazowej dla jednostek `Student` i `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="af0aa-113">In this section, you add a birth date column to the `Person` base class for the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="af0aa-114">Następnie zaktualizujesz stronę wyświetlającą dane instruktora, aby wyświetlić nową kolumnę.</span><span class="sxs-lookup"><span data-stu-id="af0aa-114">Then you update the page that displays instructor data so that it displays the new column.</span></span> <span data-ttu-id="af0aa-115">Na koniec należy wdrożyć zmiany w testach, przemieszczaniu i produkcji.</span><span class="sxs-lookup"><span data-stu-id="af0aa-115">Finally, you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-application-database"></a><span data-ttu-id="af0aa-116">Dodawanie kolumny do tabeli w bazie danych aplikacji</span><span class="sxs-lookup"><span data-stu-id="af0aa-116">Add a column to a table in the application database</span></span>

1. <span data-ttu-id="af0aa-117">W projekcie *ContosoUniversity. dal* Otwórz *Person.cs* i Dodaj następującą właściwość na końcu klasy `Person` (po niej powinny znajdować się dwa zamykające nawiasy klamrowe):</span><span class="sxs-lookup"><span data-stu-id="af0aa-117">In the *ContosoUniversity.DAL* project, open *Person.cs* and add the following property at the end of the `Person` class (there should be two closing curly braces following it):</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    <span data-ttu-id="af0aa-118">Następnie zaktualizuj metodę `Seed`, aby zapewnić wartość nowej kolumny.</span><span class="sxs-lookup"><span data-stu-id="af0aa-118">Next, update the `Seed` method so that it provides a value for the new column.</span></span> <span data-ttu-id="af0aa-119">Otwórz *Migrations\Configuration.cs* i Zastąp blok kodu, który rozpoczyna się `var instructors = new List<Instructor>` z następującym blokiem kodu, który zawiera informacje o dacie urodzenia:</span><span class="sxs-lookup"><span data-stu-id="af0aa-119">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes birth date information:</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. <span data-ttu-id="af0aa-120">Skompiluj rozwiązanie, a następnie otwórz okno **konsoli Menedżera pakietów** .</span><span class="sxs-lookup"><span data-stu-id="af0aa-120">Build the solution, and then open the **Package Manager Console** window.</span></span> <span data-ttu-id="af0aa-121">Upewnij się, że ContosoUniversity. DAL jest nadal wybrane jako **Projekt domyślny**.</span><span class="sxs-lookup"><span data-stu-id="af0aa-121">Make sure that ContosoUniversity.DAL is still selected as the **Default project**.</span></span>
3. <span data-ttu-id="af0aa-122">W oknie **konsola Menedżera pakietów** wybierz pozycję **ContosoUniversity. dal** jako **Projekt domyślny**, a następnie wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="af0aa-122">In the **Package Manager Console** window, select **ContosoUniversity.DAL** as the **Default project**, and then enter the following command:</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    <span data-ttu-id="af0aa-123">Po zakończeniu tego polecenia program Visual Studio otwiera plik klasy, który definiuje nową klasę `DbMigration`, a w metodzie `Up` można zobaczyć kod, który tworzy nową kolumnę.</span><span class="sxs-lookup"><span data-stu-id="af0aa-123">When this command finishes, Visual Studio opens the class file that defines the new `DbMigration` class, and in the `Up` method you can see the code that creates the new column.</span></span> <span data-ttu-id="af0aa-124">Metoda `Up` tworzy kolumnę podczas implementowania zmiany, a metoda `Down` usuwa kolumnę podczas wycofywania zmiany.</span><span class="sxs-lookup"><span data-stu-id="af0aa-124">The `Up` method creates the column when you are implementing the change, and the `Down` method deletes the column when you are rolling back the change.</span></span>

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. <span data-ttu-id="af0aa-126">Skompiluj rozwiązanie, a następnie wprowadź następujące polecenie w oknie **konsola Menedżera pakietów** (Upewnij się, że projekt CONTOSOUNIVERSITY. dal jest nadal zaznaczony):</span><span class="sxs-lookup"><span data-stu-id="af0aa-126">Build the solution, and then enter the following command in the **Package Manager Console** window (make sure the ContosoUniversity.DAL project is still selected):</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    <span data-ttu-id="af0aa-127">Entity Framework uruchamia metodę `Up`, a następnie uruchamia metodę `Seed`.</span><span class="sxs-lookup"><span data-stu-id="af0aa-127">The Entity Framework runs the `Up` method and then runs the `Seed` method.</span></span>

### <a name="display-the-new-column-in-the-instructors-page"></a><span data-ttu-id="af0aa-128">Wyświetlanie nowej kolumny na stronie instruktorów</span><span class="sxs-lookup"><span data-stu-id="af0aa-128">Display the new column in the Instructors page</span></span>

1. <span data-ttu-id="af0aa-129">W projekcie ContosoUniversity Otwórz program *instruktors. aspx* i Dodaj nowe pole szablonu, aby wyświetlić datę urodzenia.</span><span class="sxs-lookup"><span data-stu-id="af0aa-129">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field to display the birth date.</span></span> <span data-ttu-id="af0aa-130">Dodaj ją między tymi elementami dla daty zatrudnienia i przypisania pakietu Office:</span><span class="sxs-lookup"><span data-stu-id="af0aa-130">Add it between the ones for hire date and office assignment:</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    <span data-ttu-id="af0aa-131">(Jeśli wcięcia kodu nie są zsynchronizowane, możesz nacisnąć klawisze CTRL-K, a następnie CTRL-D, aby automatycznie ponownie sformatować plik).</span><span class="sxs-lookup"><span data-stu-id="af0aa-131">(If code indentation gets out of sync, you can press CTRL-K and then CTRL-D to automatically reformat the file.)</span></span>
2. <span data-ttu-id="af0aa-132">Uruchom aplikację, a następnie kliknij link **Instruktorzy** .</span><span class="sxs-lookup"><span data-stu-id="af0aa-132">Run the application and click the **Instructors** link.</span></span>

    <span data-ttu-id="af0aa-133">Po załadowaniu strony zobaczysz, że ma ona nowe pole Data urodzenia.</span><span class="sxs-lookup"><span data-stu-id="af0aa-133">When the page loads, you see that it has the new birth date field.</span></span>

    ![Strona instruktorów z dataurodzeniem](deploying-a-database-update/_static/image2.png)
3. <span data-ttu-id="af0aa-135">Zamknij przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="af0aa-135">Close the browser.</span></span>

### <a name="deploy-the-database-update"></a><span data-ttu-id="af0aa-136">Wdróż aktualizację bazy danych</span><span class="sxs-lookup"><span data-stu-id="af0aa-136">Deploy the database update</span></span>

1. <span data-ttu-id="af0aa-137">W **Eksplorator rozwiązań** wybierz projekt ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="af0aa-137">In **Solution Explorer** select the ContosoUniversity project.</span></span>
2. <span data-ttu-id="af0aa-138">Na pasku narzędzi **kliknij przycisk Publikuj w sieci Web** , kliknij profil publikacji **test** , a następnie kliknij pozycję **Publikuj sieć Web**.</span><span class="sxs-lookup"><span data-stu-id="af0aa-138">In the **Web One Click Publish** toolbar, click the **Test** publish profile, and then click **Publish Web**.</span></span> <span data-ttu-id="af0aa-139">(Jeśli pasek narzędzi jest wyłączony, wybierz projekt ContosoUniversity w **Eksplorator rozwiązań**).</span><span class="sxs-lookup"><span data-stu-id="af0aa-139">(If the toolbar is disabled, select the ContosoUniversity project in **Solution Explorer**.)</span></span>

    <span data-ttu-id="af0aa-140">Program Visual Studio wdroży zaktualizowaną aplikację, a w przeglądarce zostanie otwarta strona główna.</span><span class="sxs-lookup"><span data-stu-id="af0aa-140">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
3. <span data-ttu-id="af0aa-141">Uruchom stronę **instruktorów** , aby sprawdzić, czy aktualizacja została pomyślnie wdrożona.</span><span class="sxs-lookup"><span data-stu-id="af0aa-141">Run the **Instructors** page to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="af0aa-142">Gdy aplikacja próbuje uzyskać dostęp do bazy danych na tej stronie, Code First aktualizuje schemat bazy danych i uruchamia metodę `Seed`.</span><span class="sxs-lookup"><span data-stu-id="af0aa-142">When the application tries to access the database for this page, Code First updates the database schema and runs the `Seed` method.</span></span> <span data-ttu-id="af0aa-143">Gdy zostanie wyświetlona strona, zobaczysz kolumnę oczekiwanej **daty urodzenia** z datami w nim.</span><span class="sxs-lookup"><span data-stu-id="af0aa-143">When the page displays, you see the expected **Birth Date** column with dates in it.</span></span>
4. <span data-ttu-id="af0aa-144">W **sieci Web kliknij przycisk Publikuj** na pasku narzędzi, kliknij profil publikowania **przemieszczania** , a następnie kliknij przycisk **Publikuj sieć Web**.</span><span class="sxs-lookup"><span data-stu-id="af0aa-144">In the **Web One Click Publish** toolbar, click the **Staging** publish profile, and then click **Publish Web**.</span></span>
5. <span data-ttu-id="af0aa-145">Uruchom stronę **instruktorów** w obszarze przygotowanie, aby sprawdzić, czy aktualizacja została pomyślnie wdrożona.</span><span class="sxs-lookup"><span data-stu-id="af0aa-145">Run the **Instructors** page in staging to verify that the update was successfully deployed.</span></span>
6. <span data-ttu-id="af0aa-146">W **sieci Web kliknij przycisk Publikuj** na pasku narzędzi, kliknij profil publikowania **produkcyjnego** , a następnie kliknij przycisk **Publikuj sieć Web**.</span><span class="sxs-lookup"><span data-stu-id="af0aa-146">In the **Web One Click Publish** toolbar, click the **Production** publish profile, and then click **Publish Web**.</span></span>
7. <span data-ttu-id="af0aa-147">Uruchom stronę **Instruktorzy** w środowisku produkcyjnym, aby sprawdzić, czy aktualizacja została pomyślnie wdrożona.</span><span class="sxs-lookup"><span data-stu-id="af0aa-147">Run the **Instructors** page in production to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="af0aa-148">W przypadku rzeczywistej aktualizacji aplikacji produkcyjnej, która obejmuje zmianę bazy danych, przeważnie przeprowadzisz aplikację w tryb offline podczas wdrażania przy użyciu *aplikacji\_offline. htm*, jak zostało to opisane w poprzednim samouczku.</span><span class="sxs-lookup"><span data-stu-id="af0aa-148">For a real production application update that includes a database change you would also typically take the application offline during deployment by using *app\_offline.htm*, as you saw in the previous tutorial.</span></span>

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a><span data-ttu-id="af0aa-149">Wdrażanie aktualizacji bazy danych za pomocą dostawcy dbDacFx</span><span class="sxs-lookup"><span data-stu-id="af0aa-149">Deploy a database update by using the dbDacFx provider</span></span>

<span data-ttu-id="af0aa-150">W tej sekcji dodasz kolumnę *komentarzy* do tabeli *User* w bazie danych członkostwa i utworzysz stronę, która umożliwia wyświetlanie i edytowanie komentarzy dla każdego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="af0aa-150">In this section, you add a *Comments* column to the *User* table in the membership database and create a page that lets you display and edit comments for each user.</span></span> <span data-ttu-id="af0aa-151">Następnie wdrażasz zmiany w testach, przemieszczaniu i produkcji.</span><span class="sxs-lookup"><span data-stu-id="af0aa-151">Then you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-membership-database"></a><span data-ttu-id="af0aa-152">Dodawanie kolumny do tabeli w bazie danych członkostwa</span><span class="sxs-lookup"><span data-stu-id="af0aa-152">Add a column to a table in the membership database</span></span>

1. <span data-ttu-id="af0aa-153">W programie Visual Studio Otwórz **Eksplorator obiektów SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="af0aa-153">In Visual Studio, open **SQL Server Object Explorer**.</span></span>
2. <span data-ttu-id="af0aa-154">Rozwiń **(LocalDB) \v11.0**, rozwiń węzeł **bazy danych**, rozwiń pozycję **ASPNET-ContosoUniversity** (nie **ASPNET-ContosoUniversity-prod**), a następnie rozwiń węzeł **tabele**.</span><span class="sxs-lookup"><span data-stu-id="af0aa-154">Expand **(localdb)\v11.0**, expand **Databases**, expand **aspnet-ContosoUniversity** (not **aspnet-ContosoUniversity-Prod**) and then expand **Tables**.</span></span>

    <span data-ttu-id="af0aa-155">Jeśli nie widzisz **(LocalDB) \v11.0** w węźle **SQL Server** , kliknij prawym przyciskiem myszy węzeł **SQL Server** i kliknij polecenie **Dodaj SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="af0aa-155">If you don't see **(localdb)\v11.0** under the **SQL Server** node, right-click the **SQL Server** node and click **Add SQL Server**.</span></span> <span data-ttu-id="af0aa-156">W oknie dialogowym **łączenie z serwerem** wprowadź *(LocalDB) \V11.0* jako **nazwę serwera**, a następnie kliknij przycisk **Połącz**.</span><span class="sxs-lookup"><span data-stu-id="af0aa-156">In the **Connect to Server** dialog box enter *(localdb)\v11.0* as the **Server name**, and then click **Connect**.</span></span>

    <span data-ttu-id="af0aa-157">Jeśli nie widzisz **ASPNET-ContosoUniversity**, Uruchom projekt i zaloguj się przy użyciu poświadczeń *administratora* (hasło to *devpwd*), a następnie Odśwież okno **Eksplorator obiektów SQL Server** .</span><span class="sxs-lookup"><span data-stu-id="af0aa-157">If you don't see **aspnet-ContosoUniversity**, run the project and log in using the *admin* credentials (password is *devpwd*), and then refresh the **SQL Server Object Explorer** window.</span></span>
3. <span data-ttu-id="af0aa-158">Kliknij prawym przyciskiem myszy tabelę **Użytkownicy** , a następnie kliknij pozycję **Projektant widoków**.</span><span class="sxs-lookup"><span data-stu-id="af0aa-158">Right-click the **Users** table, and then click **View Designer**.</span></span>

    ![Projektant widoków SSOX](deploying-a-database-update/_static/image3.png)
4. <span data-ttu-id="af0aa-160">W projektancie Dodaj kolumnę *Komentarze* i ustaw ją jako *nvarchar (128)* i nullable, a następnie kliknij przycisk **Aktualizuj**.</span><span class="sxs-lookup"><span data-stu-id="af0aa-160">In the designer, add a *Comments* column and make it *nvarchar(128)* and nullable, and then click **Update**.</span></span>

    ![Dodawanie kolumny komentarzy](deploying-a-database-update/_static/image4.png)
5. <span data-ttu-id="af0aa-162">W oknie **Podgląd aktualizacji bazy danych** kliknij pozycję **Aktualizuj bazę danych**.</span><span class="sxs-lookup"><span data-stu-id="af0aa-162">In the **Preview Database Updates** box, click **Update Database**.</span></span>

    ![Podgląd aktualizacji bazy danych](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a><span data-ttu-id="af0aa-164">Utwórz stronę, aby wyświetlić i edytować nową kolumnę</span><span class="sxs-lookup"><span data-stu-id="af0aa-164">Create a page to display and edit the new column</span></span>

1. <span data-ttu-id="af0aa-165">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder **konta** w projekcie ContosoUniversity, kliknij polecenie **Dodaj**, a następnie kliknij pozycję **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="af0aa-165">In **Solution Explorer**, right-click the **Account** folder in the ContosoUniversity project, click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="af0aa-166">Utwórz nowy **formularz sieci Web przy użyciu strony wzorcowej** i nadaj mu nazwę *UserInfo. aspx*.</span><span class="sxs-lookup"><span data-stu-id="af0aa-166">Create a new **Web Form Using Master Page** and name it *UserInfo.aspx*.</span></span> <span data-ttu-id="af0aa-167">Zaakceptuj domyślny plik *site. Master* jako stronę wzorcową.</span><span class="sxs-lookup"><span data-stu-id="af0aa-167">Accept the default *Site.Master* file as the master page.</span></span>
3. <span data-ttu-id="af0aa-168">Skopiuj następujące znaczniki do `MainContent` elementu `Content` (ostatni z 3 elementów `Content`):</span><span class="sxs-lookup"><span data-stu-id="af0aa-168">Copy the following markup into the `MainContent` `Content` element (the last of the 3 `Content` elements):</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. <span data-ttu-id="af0aa-169">Kliknij prawym przyciskiem myszy stronę *UserInfo. aspx* , a następnie kliknij pozycję **Wyświetl w przeglądarce**.</span><span class="sxs-lookup"><span data-stu-id="af0aa-169">Right-click the *UserInfo.aspx* page and click **View in Browser**.</span></span>
5. <span data-ttu-id="af0aa-170">Zaloguj się przy użyciu poświadczeń użytkownika *administratora* (hasło to *devpwd*) i Dodaj do użytkownika pewne komentarze, aby sprawdzić, czy strona działa poprawnie.</span><span class="sxs-lookup"><span data-stu-id="af0aa-170">Log in with your *admin* user credentials (password is *devpwd*) and add some comments to a user to verify that the page works correctly.</span></span>

    ![Strona UserInfo](deploying-a-database-update/_static/image6.png)
6. <span data-ttu-id="af0aa-172">Zamknij przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="af0aa-172">Close the browser.</span></span>

## <a name="deploy-the-database-update"></a><span data-ttu-id="af0aa-173">Wdróż aktualizację bazy danych</span><span class="sxs-lookup"><span data-stu-id="af0aa-173">Deploy the database update</span></span>

<span data-ttu-id="af0aa-174">Aby wdrożyć program przy użyciu dostawcy dbDacFx, wystarczy wybrać opcję **Aktualizuj bazę danych** w profilu publikowania.</span><span class="sxs-lookup"><span data-stu-id="af0aa-174">To deploy by using the dbDacFx provider, you just need to select the **Update database** option in the publish profile.</span></span> <span data-ttu-id="af0aa-175">Jednak w przypadku wstępnego wdrożenia przy użyciu tej opcji można również skonfigurować kilka dodatkowych skryptów SQL: te są nadal w profilu i należy uniemożliwić ich ponowne uruchomienie.</span><span class="sxs-lookup"><span data-stu-id="af0aa-175">However, for the initial deployment when you used this option you also configured some additional SQL scripts to run: those are still in the profile and you'll have to prevent them from running again.</span></span>

1. <span data-ttu-id="af0aa-176">Otwórz kreatora **publikacji w sieci Web** , klikając prawym przyciskiem myszy projekt ContosoUniversity i klikając polecenie **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="af0aa-176">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="af0aa-177">Wybierz profil **testu** .</span><span class="sxs-lookup"><span data-stu-id="af0aa-177">Select the **Test** profile.</span></span>
3. <span data-ttu-id="af0aa-178">Kliknij kartę **Ustawienia** .</span><span class="sxs-lookup"><span data-stu-id="af0aa-178">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="af0aa-179">W obszarze **DefaultConnection**wybierz pozycję **Aktualizuj bazę danych**.</span><span class="sxs-lookup"><span data-stu-id="af0aa-179">Under **DefaultConnection**, select **Update database**.</span></span>
5. <span data-ttu-id="af0aa-180">Wyłącz dodatkowe skrypty skonfigurowane do uruchamiania w ramach początkowego wdrożenia:</span><span class="sxs-lookup"><span data-stu-id="af0aa-180">Disable the additional scripts that you configured to run for the initial deployment:</span></span>

    1. <span data-ttu-id="af0aa-181">Kliknij pozycję **Konfiguruj aktualizacje bazy danych**.</span><span class="sxs-lookup"><span data-stu-id="af0aa-181">Click **Configure database updates**.</span></span>
    2. <span data-ttu-id="af0aa-182">W oknie dialogowym **Konfiguruj aktualizacje bazy danych** wyczyść pola wyboru obok pozycji *Grant. SQL* i *ASPNET-Data-dev. SQL*.</span><span class="sxs-lookup"><span data-stu-id="af0aa-182">In the **Configure Database Updates** dialog box, clear the check boxes next to *Grant.sql* and *aspnet-data-dev.sql*.</span></span>
    3. <span data-ttu-id="af0aa-183">Kliknij przycisk **Zamknij**.</span><span class="sxs-lookup"><span data-stu-id="af0aa-183">Click **Close**.</span></span>
6. <span data-ttu-id="af0aa-184">Kliknij kartę **Podgląd** .</span><span class="sxs-lookup"><span data-stu-id="af0aa-184">Click the **Preview** tab.</span></span>
7. <span data-ttu-id="af0aa-185">W obszarze **bazy danych** i po prawej stronie **DefaultConnection**kliknij link **Podgląd bazy danych** .</span><span class="sxs-lookup"><span data-stu-id="af0aa-185">Under **Databases** and to the right of **DefaultConnection**, click the **Preview database** link.</span></span>

    ![Podgląd bazy danych](deploying-a-database-update/_static/image7.png)

    <span data-ttu-id="af0aa-187">W oknie podglądu zostanie wyświetlony skrypt, który zostanie uruchomiony w docelowej bazie danych, aby uczynić schemat bazy danych zgodny ze schematem źródłowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="af0aa-187">The preview window shows the script that will be run in the destination database to make that database schema match the schema of the source database.</span></span> <span data-ttu-id="af0aa-188">Skrypt zawiera polecenie ALTER TABLE, które dodaje nową kolumnę.</span><span class="sxs-lookup"><span data-stu-id="af0aa-188">The script includes an ALTER TABLE command that adds the new column.</span></span>
8. <span data-ttu-id="af0aa-189">Zamknij okno dialogowe **Podgląd bazy danych** , a następnie kliknij przycisk **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="af0aa-189">Close the **Database Preview** dialog box, and then click **Publish**.</span></span>

    <span data-ttu-id="af0aa-190">Program Visual Studio wdroży zaktualizowaną aplikację, a w przeglądarce zostanie otwarta strona główna.</span><span class="sxs-lookup"><span data-stu-id="af0aa-190">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
9. <span data-ttu-id="af0aa-191">Uruchom stronę UserInfo (Dodaj *konto/UserInfo. aspx* do adresu URL strony głównej), aby sprawdzić, czy aktualizacja została pomyślnie wdrożona.</span><span class="sxs-lookup"><span data-stu-id="af0aa-191">Run the UserInfo page (add *Account/UserInfo.aspx* to the home page URL) to verify that the update was successfully deployed.</span></span> <span data-ttu-id="af0aa-192">Musisz się zalogować, wprowadzając ciąg *admin* i *devpwd*.</span><span class="sxs-lookup"><span data-stu-id="af0aa-192">You'll have to log in by entering *admin* and *devpwd*.</span></span>

    <span data-ttu-id="af0aa-193">Dane w tabelach nie są wdrażane domyślnie, a skrypt wdrażania danych nie został skonfigurowany do uruchamiania, dlatego nie można znaleźć komentarza dodanego podczas opracowywania.</span><span class="sxs-lookup"><span data-stu-id="af0aa-193">Data in tables is not deployed by default, and you didn't configure a data deployment script to run, so you won't find the comment that you added in development.</span></span> <span data-ttu-id="af0aa-194">Możesz teraz dodać nowy komentarz w obszarze przygotowywanie, aby sprawdzić, czy zmiana została wdrożona w bazie danych, a strona działa poprawnie.</span><span class="sxs-lookup"><span data-stu-id="af0aa-194">You can add a new comment now in staging to verify that the change was deployed to the database and the page works correctly.</span></span>
10. <span data-ttu-id="af0aa-195">Postępuj zgodnie z tą samą procedurą, aby wdrożyć do etapów przejściowych i produkcyjnych.</span><span class="sxs-lookup"><span data-stu-id="af0aa-195">Follow the same procedure to deploy to staging and production.</span></span>

    <span data-ttu-id="af0aa-196">Nie zapomnij wyłączyć dodatkowych skryptów.</span><span class="sxs-lookup"><span data-stu-id="af0aa-196">Don't forget to disable the extra scripts.</span></span> <span data-ttu-id="af0aa-197">Jedyną różnicą w porównaniu z profilem testowym jest wyłączenie tylko jednego skryptu w profilach przejściowych i produkcyjnych, ponieważ zostały one skonfigurowane do uruchamiania tylko *ASPNET-prod-Data. SQL*.</span><span class="sxs-lookup"><span data-stu-id="af0aa-197">The only difference compared to the Test profile is that you will disable only one script in the Staging and Production profiles because they were configured to run only *aspnet-prod-data.sql*.</span></span>

    <span data-ttu-id="af0aa-198">Poświadczenia do przemieszczania i produkcji to admin i prodpwd.</span><span class="sxs-lookup"><span data-stu-id="af0aa-198">The credentials for staging and production are admin and prodpwd.</span></span>

    <span data-ttu-id="af0aa-199">W przypadku rzeczywistej aktualizacji aplikacji produkcyjnej, która obejmuje zmianę bazy danych, można również przełączać aplikację w tryb offline podczas wdrażania, przekazując *aplikację\_offline. htm* przed opublikowaniem i usunięciem jej później, jak pokazano w [poprzednim samouczku](deploying-a-code-update.md).</span><span class="sxs-lookup"><span data-stu-id="af0aa-199">For a real production application update that includes a database change you would also typically take the application offline during deployment by uploading *app\_offline.htm* before publishing and deleting it afterward, as you saw in [the previous tutorial](deploying-a-code-update.md).</span></span>

## <a name="summary"></a><span data-ttu-id="af0aa-200">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="af0aa-200">Summary</span></span>

<span data-ttu-id="af0aa-201">Wdrożono aktualizację aplikacji, która obejmowała zmianę bazy danych przy użyciu zarówno Migracje Code First, jak i dostawcy dbDacFx.</span><span class="sxs-lookup"><span data-stu-id="af0aa-201">You've now deployed an application update that included a database change using both Code First Migrations and the dbDacFx provider.</span></span>

![Strona instruktorów z dataurodzeniem](deploying-a-database-update/_static/image8.png)

![Strona UserInfo](deploying-a-database-update/_static/image9.png)

<span data-ttu-id="af0aa-204">W następnym samouczku pokazano, jak wykonywać wdrożenia przy użyciu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="af0aa-204">The next tutorial shows you how to execute deployments by using the command line.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="af0aa-205">[Poprzednie](deploying-a-code-update.md)
> [dalej](command-line-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="af0aa-205">[Previous](deploying-a-code-update.md)
[Next](command-line-deployment.md)</span></span>
