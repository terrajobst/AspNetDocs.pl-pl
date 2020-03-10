---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: Dodawanie modelu | Microsoft Docs
author: Rick-Anderson
description: 'Uwaga: zaktualizowana wersja tego samouczka jest dostępna w tym miejscu, w którym są używane ASP.NET MVC 5 i Visual Studio 2013. Jest to bezpieczniejsze i łatwiejsze w obserwowanie...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: a9851d93dde495814f67fa0c807d3534f5f0d8ef
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78540307"
---
# <a name="adding-a-model"></a><span data-ttu-id="ba9e6-104">Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="ba9e6-104">Adding a Model</span></span>

<span data-ttu-id="ba9e6-105">Autor [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ba9e6-105">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> > [!NOTE]
> > <span data-ttu-id="ba9e6-106">Zaktualizowana wersja tego samouczka jest dostępna w [tym miejscu](../../getting-started/introduction/getting-started.md) , w którym są używane ASP.NET MVC 5 i Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="ba9e6-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="ba9e6-107">Jest to bezpieczniejsze i łatwiejsze w obserwowanie i zademonstrowanie większej liczby funkcji.</span><span class="sxs-lookup"><span data-stu-id="ba9e6-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>

<span data-ttu-id="ba9e6-108">W tej sekcji dodasz kilka klas do zarządzania filmami w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="ba9e6-108">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="ba9e6-109">Te klasy będą modelem &quot;&quot; częścią aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="ba9e6-109">These classes will be the &quot;model&quot; part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="ba9e6-110">Będziesz używać .NET Framework technologii dostępu do danych, znanej jako [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) do definiowania i pracy z tymi klasami modelu.</span><span class="sxs-lookup"><span data-stu-id="ba9e6-110">You'll use a .NET Framework data-access technology known as the [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) to define and work with these model classes.</span></span> <span data-ttu-id="ba9e6-111">Entity Framework (często określana jako EF) obsługuje model programistyczny o nazwie *Code First*.</span><span class="sxs-lookup"><span data-stu-id="ba9e6-111">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="ba9e6-112">Code First umożliwia tworzenie obiektów modelu przez pisanie prostych klas.</span><span class="sxs-lookup"><span data-stu-id="ba9e6-112">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="ba9e6-113">(Są one również znane jako klasy POCO, od &quot;zwykłych obiektów CLR.&quot;) Następnie można utworzyć bazę danych na bieżąco od klas, co umożliwia bardzo czysty i szybki przepływ pracy programistycznej.</span><span class="sxs-lookup"><span data-stu-id="ba9e6-113">(These are also known as POCO classes, from &quot;plain-old CLR objects.&quot;) You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="ba9e6-114">Dodawanie klas modelu</span><span class="sxs-lookup"><span data-stu-id="ba9e6-114">Adding Model Classes</span></span>

<span data-ttu-id="ba9e6-115">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder *modele* , wybierz polecenie **Dodaj**, a następnie wybierz pozycję **Klasa**.</span><span class="sxs-lookup"><span data-stu-id="ba9e6-115">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="ba9e6-116">Wprowadź nazwę *klasy* &quot;filmu&quot;.</span><span class="sxs-lookup"><span data-stu-id="ba9e6-116">Enter the *class* name &quot;Movie&quot;.</span></span>

<span data-ttu-id="ba9e6-117">Dodaj następujące pięć właściwości do klasy `Movie`:</span><span class="sxs-lookup"><span data-stu-id="ba9e6-117">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="ba9e6-118">Użyjemy klasy `Movie` do reprezentowania filmów w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="ba9e6-118">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="ba9e6-119">Każde wystąpienie obiektu `Movie` będzie odpowiadać wierszowi w tabeli bazy danych, a każda właściwość klasy `Movie` zostanie zamapowana na kolumnę w tabeli.</span><span class="sxs-lookup"><span data-stu-id="ba9e6-119">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="ba9e6-120">W tym samym pliku Dodaj następującą `MovieDBContext` klasy:</span><span class="sxs-lookup"><span data-stu-id="ba9e6-120">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

<span data-ttu-id="ba9e6-121">Klasa `MovieDBContext` reprezentuje kontekst bazy danych filmów Entity Framework, który obsługuje pobieranie, przechowywanie i aktualizowanie wystąpień klasy `Movie` w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="ba9e6-121">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="ba9e6-122">`MovieDBContext` pochodzi od klasy bazowej `DbContext` dostarczonej przez Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ba9e6-122">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span>

<span data-ttu-id="ba9e6-123">Aby można było odwoływać się do `DbContext` i `DbSet`, należy dodać następującą instrukcję `using` na początku pliku:</span><span class="sxs-lookup"><span data-stu-id="ba9e6-123">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="ba9e6-124">Pełny plik *Movie.cs* jest przedstawiony poniżej.</span><span class="sxs-lookup"><span data-stu-id="ba9e6-124">The complete *Movie.cs* file is shown below.</span></span> <span data-ttu-id="ba9e6-125">(Kilka instrukcji using, które nie są konieczne, zostało usuniętych).</span><span class="sxs-lookup"><span data-stu-id="ba9e6-125">(Several using statements that are not needed have been removed.)</span></span>

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="ba9e6-126">Tworzenie parametrów połączenia i praca z bazą danych SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="ba9e6-126">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="ba9e6-127">Utworzona Klasa `MovieDBContext` obsługuje zadanie łączenia się z bazą danych i mapowania obiektów `Movie` do rekordów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ba9e6-127">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="ba9e6-128">Jednym z pytań, które można zadać, jest określenie, jak należy określić bazę danych, z którą zostanie nawiązane połączenie.</span><span class="sxs-lookup"><span data-stu-id="ba9e6-128">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="ba9e6-129">W tym celu należy dodać informacje o połączeniu w pliku *Web. config* aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ba9e6-129">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="ba9e6-130">Otwórz główny plik *Web. config* aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ba9e6-130">Open the application root *Web.config* file.</span></span> <span data-ttu-id="ba9e6-131">(Plik *Web. config* znajduje się w folderze *widoki* ). Otwórz plik *Web. config* przedstawiony na czerwono.</span><span class="sxs-lookup"><span data-stu-id="ba9e6-131">(Not the *Web.config* file in the *Views* folder.) Open the *Web.config* file outlined in red.</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="ba9e6-132">Dodaj następujące parametry połączenia do elementu `<connectionStrings>` w pliku *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="ba9e6-132">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="ba9e6-133">Poniższy przykład przedstawia część pliku *Web. config* z nowymi dodanymi parametrami połączenia:</span><span class="sxs-lookup"><span data-stu-id="ba9e6-133">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

<span data-ttu-id="ba9e6-134">Ta niewielka ilość kodu i XML to wszystko, czego potrzebujesz do reprezentowania i przechowywania danych filmowych w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="ba9e6-134">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="ba9e6-135">Następnie utworzysz nową klasę `MoviesController`, która może być używana do wyświetlania danych filmowych i umożliwia użytkownikom tworzenie nowych list filmów.</span><span class="sxs-lookup"><span data-stu-id="ba9e6-135">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ba9e6-136">[Poprzednie](adding-a-view.md)
> [dalej](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="ba9e6-136">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
