---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
title: Dodawanie modelu (C#) | Microsoft Docs
author: Rick-Anderson
description: 'Uwaga: zaktualizowana wersja tego samouczka jest dostępna w tym miejscu, w którym są używane ASP.NET MVC 5 i Visual Studio 2013. Jest to bezpieczniejsze i łatwiejsze w obserwowanie...'
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 42355b95-5f1f-413e-8d16-14cdfaaefcd8
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: a5f494eaa05bcfcd9d49873db728d71c1fd332c8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78540867"
---
# <a name="adding-a-model-c"></a><span data-ttu-id="fc951-104">Dodawanie modelu (C#)</span><span class="sxs-lookup"><span data-stu-id="fc951-104">Adding a Model (C#)</span></span>

<span data-ttu-id="fc951-105">Autor [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fc951-105">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> <span data-ttu-id="fc951-106">Ten samouczek zawiera informacje na temat tworzenia aplikacji sieci Web ASP.NET MVC przy użyciu programu Microsoft Visual Web Developer 2010 Express z dodatkiem Service Pack 1, który jest bezpłatną wersją Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fc951-106">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="fc951-107">Przed rozpoczęciem upewnij się, że zainstalowano wymagania wstępne wymienione poniżej.</span><span class="sxs-lookup"><span data-stu-id="fc951-107">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="fc951-108">Wszystkie z nich można zainstalować, klikając następujące łącze: [Instalator platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="fc951-108">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="fc951-109">Alternatywnie możesz zainstalować wstępnie wymagane składniki, korzystając z następujących linków:</span><span class="sxs-lookup"><span data-stu-id="fc951-109">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="fc951-110">Wymagania wstępne programu Visual Studio Web Developer Express SP1</span><span class="sxs-lookup"><span data-stu-id="fc951-110">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="fc951-111">Aktualizacja narzędzi ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="fc951-111">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="fc951-112">[SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(obsługa środowiska uruchomieniowego + narzędzia)</span><span class="sxs-lookup"><span data-stu-id="fc951-112">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="fc951-113">Jeśli używasz programu Visual Studio 2010 zamiast programu Visual Web Developer 2010, Zainstaluj wymagania wstępne, klikając następujące łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="fc951-113">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="fc951-114">Projekt programu Visual Web Developer z C# kodem źródłowym jest dostępny do załączenia do tego tematu.</span><span class="sxs-lookup"><span data-stu-id="fc951-114">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="fc951-115">[Pobierz wersję C# programu](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="fc951-115">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="fc951-116">Jeśli wolisz Visual Basic, przejdź do [wersji Visual Basic](../vb/adding-a-model.md) tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="fc951-116">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/adding-a-model.md) of this tutorial.</span></span>

## <a name="adding-a-model"></a><span data-ttu-id="fc951-117">Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="fc951-117">Adding a Model</span></span>

<span data-ttu-id="fc951-118">W tej sekcji dodasz kilka klas do zarządzania filmami w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="fc951-118">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="fc951-119">Te klasy będą częścią "modelem" aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="fc951-119">These classes will be the "model" part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="fc951-120">Będziesz używać .NET Framework technologii dostępu do danych, znanej jako Entity Framework do definiowania i pracy z tymi klasami modelu.</span><span class="sxs-lookup"><span data-stu-id="fc951-120">You'll use a .NET Framework data-access technology known as the Entity Framework to define and work with these model classes.</span></span> <span data-ttu-id="fc951-121">Entity Framework (często określana jako EF) obsługuje model programistyczny o nazwie *Code First*.</span><span class="sxs-lookup"><span data-stu-id="fc951-121">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="fc951-122">Code First umożliwia tworzenie obiektów modelu przez pisanie prostych klas.</span><span class="sxs-lookup"><span data-stu-id="fc951-122">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="fc951-123">(Są one również znane jako klasy POCO z "zwykłych, starych obiektów CLR"). Następnie można utworzyć bazę danych na bieżąco od klas, co umożliwia bardzo czysty i szybki przepływ pracy programistycznej.</span><span class="sxs-lookup"><span data-stu-id="fc951-123">(These are also known as POCO classes, from "plain-old CLR objects.") You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="fc951-124">Dodawanie klas modelu</span><span class="sxs-lookup"><span data-stu-id="fc951-124">Adding Model Classes</span></span>

<span data-ttu-id="fc951-125">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder *modele* , wybierz polecenie **Dodaj**, a następnie wybierz pozycję **Klasa**.</span><span class="sxs-lookup"><span data-stu-id="fc951-125">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="fc951-126">Nazwij *klasę* "Movie".</span><span class="sxs-lookup"><span data-stu-id="fc951-126">Name the *class* "Movie".</span></span>

<span data-ttu-id="fc951-127">[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="fc951-127">[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)</span></span>

<span data-ttu-id="fc951-128">Dodaj następujące pięć właściwości do klasy `Movie`:</span><span class="sxs-lookup"><span data-stu-id="fc951-128">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="fc951-129">Użyjemy klasy `Movie` do reprezentowania filmów w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="fc951-129">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="fc951-130">Każde wystąpienie obiektu `Movie` będzie odpowiadać wierszowi w tabeli bazy danych, a każda właściwość klasy `Movie` zostanie zamapowana na kolumnę w tabeli.</span><span class="sxs-lookup"><span data-stu-id="fc951-130">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="fc951-131">W tym samym pliku Dodaj następującą `MovieDBContext` klasy:</span><span class="sxs-lookup"><span data-stu-id="fc951-131">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

<span data-ttu-id="fc951-132">Klasa `MovieDBContext` reprezentuje kontekst bazy danych filmów Entity Framework, który obsługuje pobieranie, przechowywanie i aktualizowanie wystąpień klasy `Movie` w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="fc951-132">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="fc951-133">`MovieDBContext` pochodzi od klasy bazowej `DbContext` dostarczonej przez Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="fc951-133">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span> <span data-ttu-id="fc951-134">Aby uzyskać więcej informacji na temat `DbContext` i `DbSet`, zobacz [ulepszenia produktywności Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span><span class="sxs-lookup"><span data-stu-id="fc951-134">For more information about `DbContext` and `DbSet`, see [Productivity Improvements for the Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span></span>

<span data-ttu-id="fc951-135">Aby można było odwoływać się do `DbContext` i `DbSet`, należy dodać następującą instrukcję `using` na początku pliku:</span><span class="sxs-lookup"><span data-stu-id="fc951-135">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="fc951-136">Pełny plik *Movie.cs* jest przedstawiony poniżej.</span><span class="sxs-lookup"><span data-stu-id="fc951-136">The complete *Movie.cs* file is shown below.</span></span>

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a><span data-ttu-id="fc951-137">Tworzenie parametrów połączenia i praca z SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="fc951-137">Creating a Connection String and Working with SQL Server Compact</span></span>

<span data-ttu-id="fc951-138">Utworzona Klasa `MovieDBContext` obsługuje zadanie łączenia się z bazą danych i mapowania obiektów `Movie` do rekordów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="fc951-138">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="fc951-139">Jednym z pytań, które można zadać, jest określenie, jak należy określić bazę danych, z którą zostanie nawiązane połączenie.</span><span class="sxs-lookup"><span data-stu-id="fc951-139">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="fc951-140">W tym celu należy dodać informacje o połączeniu w pliku *Web. config* aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fc951-140">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="fc951-141">Otwórz główny plik *Web. config* aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fc951-141">Open the application root *Web.config* file.</span></span> <span data-ttu-id="fc951-142">(Plik *Web. config* znajduje się w folderze *widoki* ). Na poniższej ilustracji przedstawiono oba pliki *Web. config* ; Otwórz plik *Web. config* w kolorze czerwonym.</span><span class="sxs-lookup"><span data-stu-id="fc951-142">(Not the *Web.config* file in the *Views* folder.) The image below show both *Web.config* files; open the *Web.config* file circled in red.</span></span>

![](adding-a-model/_static/image4.png)

<span data-ttu-id="fc951-143">Dodaj następujące parametry połączenia do elementu `<connectionStrings>` w pliku *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="fc951-143">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="fc951-144">Poniższy przykład przedstawia część pliku *Web. config* z nowymi dodanymi parametrami połączenia:</span><span class="sxs-lookup"><span data-stu-id="fc951-144">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

<span data-ttu-id="fc951-145">Ta niewielka ilość kodu i XML to wszystko, czego potrzebujesz do reprezentowania i przechowywania danych filmowych w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="fc951-145">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="fc951-146">Następnie utworzysz nową klasę `MoviesController`, która może być używana do wyświetlania danych filmowych i umożliwia użytkownikom tworzenie nowych list filmów.</span><span class="sxs-lookup"><span data-stu-id="fc951-146">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fc951-147">[Poprzednie](adding-a-view.md)
> [dalej](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="fc951-147">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
