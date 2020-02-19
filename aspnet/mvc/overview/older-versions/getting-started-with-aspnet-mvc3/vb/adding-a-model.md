---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
title: Dodawanie modelu (VB) | Microsoft Docs
author: Rick-Anderson
description: Ten samouczek zawiera informacje na temat tworzenia aplikacji sieci Web ASP.NET MVC przy użyciu programu Microsoft Visual Web Developer 2010 Express z dodatkiem Service Pack 1, który jest...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: b3aa7720-5c78-4ca2-baef-9a52234fb7ce
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: e69d59aed4d74f08f1c653c4965b128c4dbe20ff
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457248"
---
# <a name="adding-a-model-vb"></a><span data-ttu-id="3be4b-103">Dodawanie modelu (VB)</span><span class="sxs-lookup"><span data-stu-id="3be4b-103">Adding a Model (VB)</span></span>

<span data-ttu-id="3be4b-104">Autor [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3be4b-104">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> <span data-ttu-id="3be4b-105">Ten samouczek zawiera informacje na temat tworzenia aplikacji sieci Web ASP.NET MVC przy użyciu programu Microsoft Visual Web Developer 2010 Express z dodatkiem Service Pack 1, który jest bezpłatną wersją Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3be4b-105">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="3be4b-106">Przed rozpoczęciem upewnij się, że zainstalowano wymagania wstępne wymienione poniżej.</span><span class="sxs-lookup"><span data-stu-id="3be4b-106">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="3be4b-107">Wszystkie z nich można zainstalować, klikając następujące łącze: [Instalator platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="3be4b-107">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="3be4b-108">Alternatywnie możesz zainstalować wstępnie wymagane składniki, korzystając z następujących linków:</span><span class="sxs-lookup"><span data-stu-id="3be4b-108">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="3be4b-109">Wymagania wstępne programu Visual Studio Web Developer Express SP1</span><span class="sxs-lookup"><span data-stu-id="3be4b-109">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="3be4b-110">Aktualizacja narzędzi ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="3be4b-110">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="3be4b-111">[SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(obsługa środowiska uruchomieniowego + narzędzia)</span><span class="sxs-lookup"><span data-stu-id="3be4b-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="3be4b-112">Jeśli używasz programu Visual Studio 2010 zamiast programu Visual Web Developer 2010, Zainstaluj wymagania wstępne, klikając następujące łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="3be4b-112">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="3be4b-113">Projekt Visual Web Developer z kodem źródłowym VB.NET jest dostępny do tego tematu.</span><span class="sxs-lookup"><span data-stu-id="3be4b-113">A Visual Web Developer project with VB.NET source code is available to accompany this topic.</span></span> <span data-ttu-id="3be4b-114">[Pobierz wersję VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="3be4b-114">[Download the VB.NET version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="3be4b-115">Jeśli wolisz C#, przejdź do [ C# wersji](../cs/adding-a-model.md) tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="3be4b-115">If you prefer C#, switch to the [C# version](../cs/adding-a-model.md) of this tutorial.</span></span>

## <a name="adding-a-model"></a><span data-ttu-id="3be4b-116">Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="3be4b-116">Adding a Model</span></span>

<span data-ttu-id="3be4b-117">W tej sekcji dodasz kilka klas do zarządzania filmami w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="3be4b-117">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="3be4b-118">Te klasy będą częścią "modelem" aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="3be4b-118">These classes will be the "model" part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="3be4b-119">Będziesz używać .NET Framework technologii dostępu do danych, znanej jako Entity Framework do definiowania i pracy z tymi klasami modelu.</span><span class="sxs-lookup"><span data-stu-id="3be4b-119">You'll use a .NET Framework data-access technology known as the Entity Framework to define and work with these model classes.</span></span> <span data-ttu-id="3be4b-120">Entity Framework (często określana jako EF) obsługuje model programistyczny o nazwie *Code First*.</span><span class="sxs-lookup"><span data-stu-id="3be4b-120">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="3be4b-121">Code First umożliwia tworzenie obiektów modelu przez pisanie prostych klas.</span><span class="sxs-lookup"><span data-stu-id="3be4b-121">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="3be4b-122">(Są one również znane jako klasy POCO z "zwykłych, starych obiektów CLR"). Następnie można utworzyć bazę danych na bieżąco od klas, co umożliwia bardzo czysty i szybki przepływ pracy programistycznej.</span><span class="sxs-lookup"><span data-stu-id="3be4b-122">(These are also known as POCO classes, from "plain-old CLR objects.") You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="3be4b-123">Dodawanie klas modelu</span><span class="sxs-lookup"><span data-stu-id="3be4b-123">Adding Model Classes</span></span>

<span data-ttu-id="3be4b-124">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder *modele* , wybierz polecenie **Dodaj**, a następnie wybierz pozycję **Klasa**.</span><span class="sxs-lookup"><span data-stu-id="3be4b-124">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="3be4b-125">Nazwij klasę "Movie".</span><span class="sxs-lookup"><span data-stu-id="3be4b-125">Name the class "Movie".</span></span>

<span data-ttu-id="3be4b-126">Dodaj następujące pięć właściwości do klasy `Movie`:</span><span class="sxs-lookup"><span data-stu-id="3be4b-126">Add the following five properties to the `Movie` class:</span></span>

[!code-vb[Main](adding-a-model/samples/sample1.vb)]

<span data-ttu-id="3be4b-127">Użyjemy klasy `Movie` do reprezentowania filmów w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="3be4b-127">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="3be4b-128">Każde wystąpienie obiektu `Movie` będzie odpowiadać wierszowi w tabeli bazy danych, a każda właściwość klasy `Movie` zostanie zamapowana na kolumnę w tabeli.</span><span class="sxs-lookup"><span data-stu-id="3be4b-128">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="3be4b-129">W tym samym pliku Dodaj następującą `MovieDBContext` klasy:</span><span class="sxs-lookup"><span data-stu-id="3be4b-129">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-vb[Main](adding-a-model/samples/sample2.vb)]

<span data-ttu-id="3be4b-130">Klasa `MovieDBContext` reprezentuje kontekst bazy danych filmów Entity Framework, który obsługuje pobieranie, przechowywanie i aktualizowanie wystąpień klasy `Movie` w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="3be4b-130">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="3be4b-131">`MovieDBContext` pochodzi od klasy bazowej `DbContext` dostarczonej przez Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="3be4b-131">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span> <span data-ttu-id="3be4b-132">Aby uzyskać więcej informacji na temat `DbContext` i `DbSet`, zobacz [ulepszenia produktywności Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span><span class="sxs-lookup"><span data-stu-id="3be4b-132">For more information about `DbContext` and `DbSet`, see [Productivity Improvements for the Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span></span>

<span data-ttu-id="3be4b-133">Aby można było odwoływać się do `DbContext` i `DbSet`, należy dodać następującą instrukcję `imports` na początku pliku:</span><span class="sxs-lookup"><span data-stu-id="3be4b-133">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `imports` statement at the top of the file:</span></span>

[!code-vb[Main](adding-a-model/samples/sample3.vb)]

<span data-ttu-id="3be4b-134">Pełny plik *film. vb* jest przedstawiony poniżej.</span><span class="sxs-lookup"><span data-stu-id="3be4b-134">The complete *Movie.vb* file is shown below.</span></span>

[!code-vb[Main](adding-a-model/samples/sample4.vb)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a><span data-ttu-id="3be4b-135">Tworzenie parametrów połączenia i praca z SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="3be4b-135">Creating a Connection String and Working with SQL Server Compact</span></span>

<span data-ttu-id="3be4b-136">Utworzona Klasa `MovieDBContext` obsługuje zadanie łączenia się z bazą danych i mapowania obiektów `Movie` do rekordów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3be4b-136">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="3be4b-137">Jednym z pytań, które można zadać, jest określenie, jak należy określić bazę danych, z którą zostanie nawiązane połączenie.</span><span class="sxs-lookup"><span data-stu-id="3be4b-137">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="3be4b-138">W tym celu należy dodać informacje o połączeniu w pliku *Web. config* aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3be4b-138">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="3be4b-139">Otwórz główny plik *Web. config* aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3be4b-139">Open the application root *Web.config* file.</span></span> <span data-ttu-id="3be4b-140">(Plik *Web. config* znajduje się w folderze *widoki* ). Na poniższej ilustracji przedstawiono oba pliki *Web. config* ; Otwórz plik *Web. config* w kolorze czerwonym.</span><span class="sxs-lookup"><span data-stu-id="3be4b-140">(Not the *Web.config* file in the *Views* folder.) The image below show both *Web.config* files; open the *Web.config* file circled in red.</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="3be4b-141">Dodaj następujące parametry połączenia do elementu `<connectionStrings>` w pliku *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="3be4b-141">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="3be4b-142">Poniższy przykład przedstawia część pliku *Web. config* z nowymi dodanymi parametrami połączenia:</span><span class="sxs-lookup"><span data-stu-id="3be4b-142">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

<span data-ttu-id="3be4b-143">Ta niewielka ilość kodu i XML to wszystko, czego potrzebujesz do reprezentowania i przechowywania danych filmowych w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="3be4b-143">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="3be4b-144">Następnie utworzysz nową klasę `MoviesController`, która może być używana do wyświetlania danych filmowych i umożliwia użytkownikom tworzenie nowych list filmów.</span><span class="sxs-lookup"><span data-stu-id="3be4b-144">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3be4b-145">[Poprzednie](adding-a-view.md)
> [dalej](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="3be4b-145">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
