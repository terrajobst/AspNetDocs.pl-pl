---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: Użyj Migracje Code First do wypełniania bazy danych | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: 257bd06848adb949330856cc71eeb3d685e9d036
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557457"
---
# <a name="use-code-first-migrations-to-seed-the-database"></a><span data-ttu-id="4e5d8-102">Używanie Migracje Code First do wypełniania bazy danych</span><span class="sxs-lookup"><span data-stu-id="4e5d8-102">Use Code First Migrations to Seed the Database</span></span>

<span data-ttu-id="4e5d8-103">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="4e5d8-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="4e5d8-104">Pobierz ukończony projekt</span><span class="sxs-lookup"><span data-stu-id="4e5d8-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="4e5d8-105">W tej sekcji użyjesz [migracje Code First](https://msdn.microsoft.com/data/jj591621) w EF do wypełniania bazy danych danymi testowymi.</span><span class="sxs-lookup"><span data-stu-id="4e5d8-105">In this section, you will use [Code First Migrations](https://msdn.microsoft.com/data/jj591621) in EF to seed the database with test data.</span></span>

<span data-ttu-id="4e5d8-106">W menu **Narzędzia** wybierz pozycję **Menedżer pakietów NuGet**, a następnie wybierz pozycję **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="4e5d8-106">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="4e5d8-107">W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="4e5d8-107">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](part-3/samples/sample1.cmd)]

<span data-ttu-id="4e5d8-108">To polecenie dodaje folder o nazwie migrations do projektu oraz plik kodu o nazwie Configuration.cs w folderze migrations.</span><span class="sxs-lookup"><span data-stu-id="4e5d8-108">This command adds a folder named Migrations to your project, plus a code file named Configuration.cs in the Migrations folder.</span></span>

![](part-3/_static/image1.png)

<span data-ttu-id="4e5d8-109">Otwórz plik Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="4e5d8-109">Open the Configuration.cs file.</span></span> <span data-ttu-id="4e5d8-110">Dodaj następującą instrukcję **using** .</span><span class="sxs-lookup"><span data-stu-id="4e5d8-110">Add the following **using** statement.</span></span>

[!code-csharp[Main](part-3/samples/sample2.cs)]

<span data-ttu-id="4e5d8-111">Następnie Dodaj następujący kod do metody **Configuration. inicjator** :</span><span class="sxs-lookup"><span data-stu-id="4e5d8-111">Then add the following code to the **Configuration.Seed** method:</span></span>

[!code-csharp[Main](part-3/samples/sample3.cs)]

<span data-ttu-id="4e5d8-112">W oknie Konsola Menedżera pakietów wpisz następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="4e5d8-112">In the Package Manager Console window, type the following commands:</span></span>

[!code-console[Main](part-3/samples/sample4.cmd)]

<span data-ttu-id="4e5d8-113">Pierwsze polecenie generuje kod, który tworzy bazę danych, a drugie polecenie wykonuje ten kod.</span><span class="sxs-lookup"><span data-stu-id="4e5d8-113">The first command generates code that creates the database, and the second command executes that code.</span></span> <span data-ttu-id="4e5d8-114">Baza danych jest tworzona lokalnie, przy użyciu [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).</span><span class="sxs-lookup"><span data-stu-id="4e5d8-114">The database is created locally, using [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).</span></span>

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a><span data-ttu-id="4e5d8-115">Eksplorowanie interfejsu API (opcjonalnie)</span><span class="sxs-lookup"><span data-stu-id="4e5d8-115">Explore the API (Optional)</span></span>

<span data-ttu-id="4e5d8-116">Naciśnij klawisz F5, aby uruchomić aplikację w trybie debugowania.</span><span class="sxs-lookup"><span data-stu-id="4e5d8-116">Press F5 to run the application in debug mode.</span></span> <span data-ttu-id="4e5d8-117">Program Visual Studio uruchamia się IIS Express i uruchamia aplikację sieci Web.</span><span class="sxs-lookup"><span data-stu-id="4e5d8-117">Visual Studio starts IIS Express and runs your web app.</span></span> <span data-ttu-id="4e5d8-118">Program Visual Studio uruchomi przeglądarkę i otworzy stronę główną aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4e5d8-118">Visual Studio then launches a browser and opens the app's home page.</span></span>

<span data-ttu-id="4e5d8-119">Gdy program Visual Studio uruchamia projekt sieci Web, przypisuje numer portu.</span><span class="sxs-lookup"><span data-stu-id="4e5d8-119">When Visual Studio runs a web project, it assigns a port number.</span></span> <span data-ttu-id="4e5d8-120">Na poniższej ilustracji numer portu to 50524.</span><span class="sxs-lookup"><span data-stu-id="4e5d8-120">In the image below, the port number is 50524.</span></span> <span data-ttu-id="4e5d8-121">Po uruchomieniu aplikacji zobaczysz inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="4e5d8-121">When you run the application, you'll see a different port number.</span></span>

![](part-3/_static/image3.png)

<span data-ttu-id="4e5d8-122">Strona główna jest implementowana przy użyciu ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="4e5d8-122">The home page is implemented using ASP.NET MVC.</span></span> <span data-ttu-id="4e5d8-123">W górnej części strony znajduje się link "API".</span><span class="sxs-lookup"><span data-stu-id="4e5d8-123">At the top of the page, there is a link that says "API".</span></span> <span data-ttu-id="4e5d8-124">Ten link umożliwia przełączenie do automatycznie generowanej strony pomocy dla internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="4e5d8-124">This link brings you to an auto-generated help page for the web API.</span></span> <span data-ttu-id="4e5d8-125">(Aby dowiedzieć się, jak ta strona pomocy jest generowana i jak można dodać własną dokumentację do strony, zobacz [Tworzenie stron pomocy dla interfejsu API sieci Web ASP.NET](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md)). Możesz kliknąć linki na stronie pomocy, aby wyświetlić szczegółowe informacje o interfejsie API, w tym format żądania i odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="4e5d8-125">(To learn how this help page is generated, and how you can add your own documentation to the page, see [Creating Help Pages for ASP.NET Web API](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) You can click on the help page links to see details about the API, including the request and response format.</span></span>

![](part-3/_static/image4.png)

<span data-ttu-id="4e5d8-126">Interfejs API umożliwia wykonywanie operacji CRUD w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="4e5d8-126">The API enables CRUD operations on the database.</span></span> <span data-ttu-id="4e5d8-127">Poniżej przedstawiono podsumowanie interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="4e5d8-127">The following summarizes the API.</span></span>

| <span data-ttu-id="4e5d8-128">Autorzy</span><span class="sxs-lookup"><span data-stu-id="4e5d8-128">Authors</span></span> |  |
| --- | -- |
| <span data-ttu-id="4e5d8-129">GET api/authors</span><span class="sxs-lookup"><span data-stu-id="4e5d8-129">GET api/authors</span></span> | <span data-ttu-id="4e5d8-130">Pobierz wszystkich autorów.</span><span class="sxs-lookup"><span data-stu-id="4e5d8-130">Get all authors.</span></span> |
| <span data-ttu-id="4e5d8-131">GET api/authors/{id}</span><span class="sxs-lookup"><span data-stu-id="4e5d8-131">GET api/authors/{id}</span></span> | <span data-ttu-id="4e5d8-132">Pobierz autora według identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="4e5d8-132">Get an author by ID.</span></span> |
| <span data-ttu-id="4e5d8-133">POST /api/authors</span><span class="sxs-lookup"><span data-stu-id="4e5d8-133">POST /api/authors</span></span> | <span data-ttu-id="4e5d8-134">Utwórz nowego autora.</span><span class="sxs-lookup"><span data-stu-id="4e5d8-134">Create a new author.</span></span> |
| <span data-ttu-id="4e5d8-135">PUT /api/authors/{id}</span><span class="sxs-lookup"><span data-stu-id="4e5d8-135">PUT /api/authors/{id}</span></span> | <span data-ttu-id="4e5d8-136">Zaktualizuj istniejącego autora.</span><span class="sxs-lookup"><span data-stu-id="4e5d8-136">Update an existing author.</span></span> |
| <span data-ttu-id="4e5d8-137">DELETE /api/authors/{id}</span><span class="sxs-lookup"><span data-stu-id="4e5d8-137">DELETE /api/authors/{id}</span></span> | <span data-ttu-id="4e5d8-138">Usuń autora.</span><span class="sxs-lookup"><span data-stu-id="4e5d8-138">Delete an author.</span></span> |

| <span data-ttu-id="4e5d8-139">Książki</span><span class="sxs-lookup"><span data-stu-id="4e5d8-139">Books</span></span> |  |
| --- | -- |
| <span data-ttu-id="4e5d8-140">GET /api/books</span><span class="sxs-lookup"><span data-stu-id="4e5d8-140">GET /api/books</span></span> | <span data-ttu-id="4e5d8-141">Pobierz wszystkie książki.</span><span class="sxs-lookup"><span data-stu-id="4e5d8-141">Get all books.</span></span> |
| <span data-ttu-id="4e5d8-142">GET /api/books/{id}</span><span class="sxs-lookup"><span data-stu-id="4e5d8-142">GET /api/books/{id}</span></span> | <span data-ttu-id="4e5d8-143">Pobierz książkę według identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="4e5d8-143">Get a book by ID.</span></span> |
| <span data-ttu-id="4e5d8-144">POST /api/books</span><span class="sxs-lookup"><span data-stu-id="4e5d8-144">POST /api/books</span></span> | <span data-ttu-id="4e5d8-145">Utwórz nową książkę.</span><span class="sxs-lookup"><span data-stu-id="4e5d8-145">Create a new book.</span></span> |
| <span data-ttu-id="4e5d8-146">PUT /api/books/{id}</span><span class="sxs-lookup"><span data-stu-id="4e5d8-146">PUT /api/books/{id}</span></span> | <span data-ttu-id="4e5d8-147">Aktualizowanie istniejącej książki.</span><span class="sxs-lookup"><span data-stu-id="4e5d8-147">Update an existing book.</span></span> |
| <span data-ttu-id="4e5d8-148">DELETE /api/books/{id}</span><span class="sxs-lookup"><span data-stu-id="4e5d8-148">DELETE /api/books/{id}</span></span> | <span data-ttu-id="4e5d8-149">Usuń książkę.</span><span class="sxs-lookup"><span data-stu-id="4e5d8-149">Delete a book.</span></span> |

## <a name="view-the-database-optional"></a><span data-ttu-id="4e5d8-150">Wyświetl bazę danych (opcjonalnie)</span><span class="sxs-lookup"><span data-stu-id="4e5d8-150">View the Database (Optional)</span></span>

<span data-ttu-id="4e5d8-151">Po uruchomieniu polecenia Update-Database, EF utworzył bazę danych i wywołała metodę `Seed`.</span><span class="sxs-lookup"><span data-stu-id="4e5d8-151">When you ran the Update-Database command, EF created the database and called the `Seed` method.</span></span> <span data-ttu-id="4e5d8-152">Gdy aplikacja jest uruchamiana lokalnie, EF używa [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx).</span><span class="sxs-lookup"><span data-stu-id="4e5d8-152">When you run the application locally, EF uses [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx).</span></span> <span data-ttu-id="4e5d8-153">Bazę danych można wyświetlić w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4e5d8-153">You can view the database in Visual Studio.</span></span> <span data-ttu-id="4e5d8-154">Z menu **Widok** wybierz opcję **Eksplorator obiektów SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="4e5d8-154">From the **View** menu, select **SQL Server Object Explorer**.</span></span>

![](part-3/_static/image5.png)

<span data-ttu-id="4e5d8-155">W oknie dialogowym **łączenie z serwerem** w polu **Nazwa serwera** wpisz "(LocalDB) \v11.0".</span><span class="sxs-lookup"><span data-stu-id="4e5d8-155">In the **Connect to Server** dialog, in the **Server Name** edit box, type "(localdb)\v11.0".</span></span> <span data-ttu-id="4e5d8-156">Pozostaw opcję **uwierzytelniania** "uwierzytelnianie systemu Windows".</span><span class="sxs-lookup"><span data-stu-id="4e5d8-156">Leave the **Authentication** option as "Windows Authentication".</span></span> <span data-ttu-id="4e5d8-157">Kliknij przycisk **Connect** (Połącz).</span><span class="sxs-lookup"><span data-stu-id="4e5d8-157">Click **Connect**.</span></span>

![](part-3/_static/image6.png)

<span data-ttu-id="4e5d8-158">Program Visual Studio nawiązuje połączenie z usługą LocalDB i wyświetla istniejące bazy danych w oknie Eksplorator obiektów SQL Server.</span><span class="sxs-lookup"><span data-stu-id="4e5d8-158">Visual Studio connects to LocalDB and shows your existing databases in the SQL Server Object Explorer window.</span></span> <span data-ttu-id="4e5d8-159">Można rozwinąć węzły, aby wyświetlić tabele, które zostały utworzone przez EF.</span><span class="sxs-lookup"><span data-stu-id="4e5d8-159">You can expand the nodes to see the tables that EF created.</span></span>

![](part-3/_static/image7.png)

<span data-ttu-id="4e5d8-160">Aby wyświetlić dane, kliknij prawym przyciskiem myszy tabelę i wybierz polecenie **Wyświetl dane**.</span><span class="sxs-lookup"><span data-stu-id="4e5d8-160">To view the data, right-click a table and select **View Data**.</span></span>

![](part-3/_static/image8.png)

<span data-ttu-id="4e5d8-161">Poniższy zrzut ekranu przedstawia wyniki tabeli Books.</span><span class="sxs-lookup"><span data-stu-id="4e5d8-161">The following screenshot shows the results for the Books table.</span></span> <span data-ttu-id="4e5d8-162">Zwróć uwagę, że EF zapełnił bazę danych danymi o inicjatorze, a tabela zawiera klucz obcy do tabeli autorów.</span><span class="sxs-lookup"><span data-stu-id="4e5d8-162">Notice that EF populated the database with the seed data, and the table contains the foreign key to the Authors table.</span></span>

![](part-3/_static/image9.png)

> [!div class="step-by-step"]
> <span data-ttu-id="4e5d8-163">[Poprzednie](part-2.md)
> [dalej](part-4.md)</span><span class="sxs-lookup"><span data-stu-id="4e5d8-163">[Previous](part-2.md)
[Next](part-4.md)</span></span>
