---
uid: mvc/overview/getting-started/introduction/creating-a-connection-string
title: Tworzenie parametrów połączenia i praca z SQL Server LocalDB | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 6127804d-c1a9-414d-8429-7f3dd0f56e97
msc.legacyurl: /mvc/overview/getting-started/introduction/creating-a-connection-string
msc.type: authoredcontent
ms.openlocfilehash: 20781ad760d3a0e4559ec4c7e18528f3686dcc02
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78615669"
---
# <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="1f68e-102">Tworzenie parametrów połączenia i praca z bazą danych SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="1f68e-102">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="1f68e-103">Autor [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1f68e-103">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [Tutorial Note](index.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="1f68e-104">Tworzenie parametrów połączenia i praca z bazą danych SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="1f68e-104">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="1f68e-105">Utworzona Klasa `MovieDBContext` obsługuje zadanie łączenia się z bazą danych i mapowania obiektów `Movie` do rekordów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1f68e-105">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="1f68e-106">Jednym z pytań, które można zadać, jest określenie, jak należy określić bazę danych, z którą zostanie nawiązane połączenie.</span><span class="sxs-lookup"><span data-stu-id="1f68e-106">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="1f68e-107">Nie trzeba określać bazy danych, która ma być używana, Entity Framework domyślnie będzie używać [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span><span class="sxs-lookup"><span data-stu-id="1f68e-107">You don't actually have to specify which database to use, Entity Framework will default to using [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> <span data-ttu-id="1f68e-108">W tej sekcji jawnie dodamy parametry połączenia w pliku *Web. config* aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1f68e-108">In this section we'll explicitly add a connection string in the *Web.config* file of the application.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="1f68e-109">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="1f68e-109">SQL Server Express LocalDB</span></span>

<span data-ttu-id="1f68e-110">[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) to uproszczona wersja aparatu bazy danych SQL Server Express uruchamiana na żądanie i uruchamiana w trybie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="1f68e-110">[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) is a lightweight version of the SQL Server Express Database Engine that starts on demand and runs in user mode.</span></span> <span data-ttu-id="1f68e-111">LocalDB działa w specjalnym trybie wykonywania SQL Server Express, który umożliwia współpracę z bazami danych jako plikami *MDF* .</span><span class="sxs-lookup"><span data-stu-id="1f68e-111">LocalDB runs in a special execution mode of SQL Server Express that enables you to work with databases as *.mdf* files.</span></span> <span data-ttu-id="1f68e-112">Zazwyczaj pliki bazy danych LocalDB są przechowywane w folderze *danych\_aplikacji* projektu sieci Web.</span><span class="sxs-lookup"><span data-stu-id="1f68e-112">Typically, LocalDB database files are kept in the *App\_Data* folder of a web project.</span></span>

<span data-ttu-id="1f68e-113">Nie zaleca się stosowania SQL Server Express w produkcyjnych aplikacjach sieci Web.</span><span class="sxs-lookup"><span data-stu-id="1f68e-113">SQL Server Express is not recommended for use in production web applications.</span></span> <span data-ttu-id="1f68e-114">LocalDB w szczególności nie należy używać w środowisku produkcyjnym z aplikacją sieci Web, ponieważ nie jest ona przeznaczona do pracy z usługami IIS.</span><span class="sxs-lookup"><span data-stu-id="1f68e-114">LocalDB in particular should not be used for production with a web application because it is not designed to work with IIS.</span></span> <span data-ttu-id="1f68e-115">Jednak baza danych LocalDB może być łatwo migrowana do SQL Server lub SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="1f68e-115">However, a LocalDB database can be easily migrated to SQL Server or SQL Azure.</span></span>

<span data-ttu-id="1f68e-116">W programie Visual Studio 2017 LocalDB jest instalowany domyślnie z programem Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1f68e-116">In Visual Studio 2017, LocalDB is installed by default with Visual Studio.</span></span>

<span data-ttu-id="1f68e-117">Domyślnie Entity Framework szuka parametrów połączenia o nazwie identycznej z klasą kontekstu obiektu (`MovieDBContext` dla tego projektu).</span><span class="sxs-lookup"><span data-stu-id="1f68e-117">By default, the Entity Framework looks for a connection string named the same as the object context class (`MovieDBContext` for this project).</span></span> <span data-ttu-id="1f68e-118">Aby uzyskać więcej informacji, zobacz [SQL Server parametry połączenia dla aplikacji sieci Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx).</span><span class="sxs-lookup"><span data-stu-id="1f68e-118">For more information see [SQL Server Connection Strings for ASP.NET Web Applications](https://msdn.microsoft.com/library/jj653752.aspx).</span></span>

<span data-ttu-id="1f68e-119">Otwórz główny plik *Web. config* aplikacji przedstawiony poniżej.</span><span class="sxs-lookup"><span data-stu-id="1f68e-119">Open the application root *Web.config* file shown below.</span></span> <span data-ttu-id="1f68e-120">(Plik *Web. config* znajduje się w folderze *widoki* ).</span><span class="sxs-lookup"><span data-stu-id="1f68e-120">(Not the *Web.config* file in the *Views* folder.)</span></span>

![](creating-a-connection-string/_static/image1.png)

<span data-ttu-id="1f68e-121">Znajdź `<connectionStrings>` element:</span><span class="sxs-lookup"><span data-stu-id="1f68e-121">Find the `<connectionStrings>` element:</span></span>

![](creating-a-connection-string/_static/image2.png)

<span data-ttu-id="1f68e-122">Dodaj następujące parametry połączenia do elementu `<connectionStrings>` w pliku *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="1f68e-122">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

<span data-ttu-id="1f68e-123">Poniższy przykład przedstawia część pliku *Web. config* z nowymi dodanymi parametrami połączenia:</span><span class="sxs-lookup"><span data-stu-id="1f68e-123">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

<span data-ttu-id="1f68e-124">Dwa parametry połączenia są bardzo podobne.</span><span class="sxs-lookup"><span data-stu-id="1f68e-124">The two connection strings are very similar.</span></span> <span data-ttu-id="1f68e-125">Pierwsze parametry połączenia mają nazwę `DefaultConnection` i służy do kontrolowania, kto może uzyskać dostęp do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1f68e-125">The first connection string is named `DefaultConnection` and is used for the membership database to control who can access the application.</span></span> <span data-ttu-id="1f68e-126">Dodane parametry połączenia określają LocalDB bazę danych o nazwie *Movie. mdf* znajdującą się w folderze *danych\_aplikacji* .</span><span class="sxs-lookup"><span data-stu-id="1f68e-126">The connection string you've added specifies a LocalDB database named *Movie.mdf* located in the *App\_Data* folder.</span></span> <span data-ttu-id="1f68e-127">W tym samouczku nie będziemy korzystać z bazy danych członkostwa, aby uzyskać więcej informacji na temat członkostwa, uwierzytelniania i zabezpieczeń, zobacz mój samouczek [Tworzenie aplikacji ASP.NET MVC z uwierzytelnianiem i bazą danych SQL, a następnie wdrażanie jej w Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="1f68e-127">We won't use the membership database in this tutorial, for more information on membership, authentication and security, see my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span>

<span data-ttu-id="1f68e-128">Nazwa parametrów połączenia musi być zgodna z nazwą klasy [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) .</span><span class="sxs-lookup"><span data-stu-id="1f68e-128">The name of the connection string must match the name of the [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) class.</span></span>

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

<span data-ttu-id="1f68e-129">Nie trzeba faktycznie dodawać parametrów połączenia `MovieDBContext`.</span><span class="sxs-lookup"><span data-stu-id="1f68e-129">You don't actually need to add the `MovieDBContext` connection string.</span></span> <span data-ttu-id="1f68e-130">Jeśli nie określisz parametrów połączenia, Entity Framework utworzy bazę danych LocalDB w katalogu Users z w pełni kwalifikowaną nazwą klasy [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) (w tym przypadku `MvcMovie.Models.MovieDBContext`).</span><span class="sxs-lookup"><span data-stu-id="1f68e-130">If you don't specify a connection string, Entity Framework will create a LocalDB database in the users directory with the fully qualified name of the [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) class (in this case `MvcMovie.Models.MovieDBContext`).</span></span> <span data-ttu-id="1f68e-131">Możesz nazywać dowolną bazę danych, o ile ma *. Sufiks MDF* .</span><span class="sxs-lookup"><span data-stu-id="1f68e-131">You can name the database anything you like, as long as it has the *.MDF* suffix.</span></span> <span data-ttu-id="1f68e-132">Załóżmy na przykład, że nazwa bazy danych to *. mdf*.</span><span class="sxs-lookup"><span data-stu-id="1f68e-132">For example, we could name the database *MyFilms.mdf*.</span></span>

<span data-ttu-id="1f68e-133">Następnie utworzysz nową klasę `MoviesController`, która może być używana do wyświetlania danych filmowych i umożliwia użytkownikom tworzenie nowych list filmów.</span><span class="sxs-lookup"><span data-stu-id="1f68e-133">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1f68e-134">[Poprzednie](adding-a-model.md)
> [dalej](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="1f68e-134">[Previous](adding-a-model.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
