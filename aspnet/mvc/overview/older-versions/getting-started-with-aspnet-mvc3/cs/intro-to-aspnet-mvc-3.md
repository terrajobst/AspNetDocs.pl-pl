---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
title: Wprowadzenie do ASP.NET MVC 3 (C#) | Microsoft Docs
author: Rick-Anderson
description: Ten samouczek zawiera informacje na temat tworzenia aplikacji sieci Web ASP.NET MVC przy użyciu programu Microsoft Visual Web Developer 2010 Express z dodatkiem Service Pack 1, który jest...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 86a80b35-88bd-4b7c-bd58-f6e7997197d4
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: e71275c93558c0b6ca087a145786e8c846b69721
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457541"
---
# <a name="intro-to-aspnet-mvc-3-c"></a><span data-ttu-id="ca590-103">Wprowadzenie do wzorca ASP.NET MVC 3 (C#)</span><span class="sxs-lookup"><span data-stu-id="ca590-103">Intro to ASP.NET MVC 3 (C#)</span></span>

<span data-ttu-id="ca590-104">Autor [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ca590-104">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> > [!NOTE]
> > <span data-ttu-id="ca590-105">Zaktualizowana wersja tego samouczka jest dostępna w [tym miejscu](../../../getting-started/introduction/getting-started.md) , w którym są używane ASP.NET MVC 5 i Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="ca590-105">An updated version of this tutorial is available [here](../../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="ca590-106">Jest to bezpieczniejsze i łatwiejsze w obserwowanie i zademonstrowanie większej liczby funkcji.</span><span class="sxs-lookup"><span data-stu-id="ca590-106">It's more secure, much simpler to follow and demonstrates more features.</span></span>
> 
> 
> <span data-ttu-id="ca590-107">Ten samouczek zawiera informacje na temat tworzenia aplikacji sieci Web ASP.NET MVC przy użyciu programu Microsoft Visual Web Developer 2010 Express z dodatkiem Service Pack 1, który jest bezpłatną wersją Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ca590-107">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="ca590-108">Przed rozpoczęciem upewnij się, że zainstalowano wymagania wstępne wymienione poniżej.</span><span class="sxs-lookup"><span data-stu-id="ca590-108">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="ca590-109">Wszystkie z nich można zainstalować, klikając następujące łącze: [Instalator platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="ca590-109">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="ca590-110">Alternatywnie możesz zainstalować wstępnie wymagane składniki, korzystając z następujących linków:</span><span class="sxs-lookup"><span data-stu-id="ca590-110">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="ca590-111">Wymagania wstępne programu Visual Studio Web Developer Express SP1</span><span class="sxs-lookup"><span data-stu-id="ca590-111">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="ca590-112">Aktualizacja narzędzi ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="ca590-112">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="ca590-113">[SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(obsługa środowiska uruchomieniowego + narzędzia)</span><span class="sxs-lookup"><span data-stu-id="ca590-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="ca590-114">Jeśli używasz programu Visual Studio 2010 zamiast programu Visual Web Developer 2010, Zainstaluj wymagania wstępne, klikając następujące łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="ca590-114">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="ca590-115">Projekt programu Visual Web Developer z C# kodem źródłowym jest dostępny do załączenia do tego tematu.</span><span class="sxs-lookup"><span data-stu-id="ca590-115">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="ca590-116">[Pobierz wersję C# programu](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="ca590-116">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="ca590-117">Jeśli wolisz Visual Basic, przejdź do [wersji Visual Basic](../vb/intro-to-aspnet-mvc-3.md) tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="ca590-117">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="ca590-118">Co będziesz kompilować</span><span class="sxs-lookup"><span data-stu-id="ca590-118">What You'll Build</span></span>

<span data-ttu-id="ca590-119">Zaimplementujmy prostą aplikację z listą filmów, która obsługuje tworzenie, edytowanie i wyświetlanie list filmów z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ca590-119">You'll implement a simple movie-listing application that supports creating, editing, and listing movies from a database.</span></span> <span data-ttu-id="ca590-120">Poniżej znajdują się dwa zrzuty ekranu aplikacji, która zostanie utworzona.</span><span class="sxs-lookup"><span data-stu-id="ca590-120">Below are two screenshots of the application you'll build.</span></span> <span data-ttu-id="ca590-121">Zawiera stronę wyświetlającą listę filmów z bazy danych:</span><span class="sxs-lookup"><span data-stu-id="ca590-121">It includes a page that displays a list of movies from a database:</span></span>

![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image1.png)

<span data-ttu-id="ca590-123">Aplikacja umożliwia także dodawanie, edytowanie i usuwanie filmów oraz wyświetlanie szczegółowych informacji o poszczególnych użytkownikach.</span><span class="sxs-lookup"><span data-stu-id="ca590-123">The application also lets you add, edit, and delete movies, as well as see details about individual ones.</span></span> <span data-ttu-id="ca590-124">Wszystkie scenariusze związane z wprowadzaniem danych obejmują sprawdzanie poprawności, aby upewnić się, że dane przechowywane w bazie danych są poprawne.</span><span class="sxs-lookup"><span data-stu-id="ca590-124">All data-entry scenarios include validation to ensure that the data stored in the database is correct.</span></span>

![](intro-to-aspnet-mvc-3/_static/image2.png)

## <a name="skills-youll-learn"></a><span data-ttu-id="ca590-125">Posiadane umiejętności</span><span class="sxs-lookup"><span data-stu-id="ca590-125">Skills You'll Learn</span></span>

<span data-ttu-id="ca590-126">Oto, co uzyskasz:</span><span class="sxs-lookup"><span data-stu-id="ca590-126">Here's what you'll learn:</span></span>

- <span data-ttu-id="ca590-127">Jak utworzyć nowy projekt ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="ca590-127">How to create a new ASP.NET MVC project.</span></span>
- <span data-ttu-id="ca590-128">Jak utworzyć kontrolery i widoki ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="ca590-128">How to create ASP.NET MVC controllers and views.</span></span>
- <span data-ttu-id="ca590-129">Jak utworzyć nową bazę danych przy użyciu Entity Framework Code First model.</span><span class="sxs-lookup"><span data-stu-id="ca590-129">How to create a new database using the Entity Framework Code First paradigm.</span></span>
- <span data-ttu-id="ca590-130">Jak pobierać i wyświetlać dane.</span><span class="sxs-lookup"><span data-stu-id="ca590-130">How to retrieve and display data.</span></span>
- <span data-ttu-id="ca590-131">Jak edytować dane i włączać sprawdzanie poprawności danych.</span><span class="sxs-lookup"><span data-stu-id="ca590-131">How to edit data and enable data validation.</span></span>

## <a name="getting-started"></a><span data-ttu-id="ca590-132">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="ca590-132">Getting Started</span></span>

<span data-ttu-id="ca590-133">Zacznij od uruchomienia programu Visual Web Developer 2010 Express ("Visual Web Developer" for Short) i wybierz pozycję **Nowy projekt** na stronie **początkowej** .</span><span class="sxs-lookup"><span data-stu-id="ca590-133">Start by running Visual Web Developer 2010 Express ("Visual Web Developer" for short) and select **New Project** from the **Start** page.</span></span>

<span data-ttu-id="ca590-134">Visual Web Developer to IDE lub zintegrowane środowisko programistyczne.</span><span class="sxs-lookup"><span data-stu-id="ca590-134">Visual Web Developer is an IDE, or integrated development environment.</span></span> <span data-ttu-id="ca590-135">Podobnie jak w przypadku używania programu Microsoft Word do pisania dokumentów, do tworzenia aplikacji będziesz używać środowiska IDE.</span><span class="sxs-lookup"><span data-stu-id="ca590-135">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="ca590-136">W programie Visual Web Developer znajduje się pasek narzędzi wzdłuż górnej krawędzi, w którym są dostępne różne opcje.</span><span class="sxs-lookup"><span data-stu-id="ca590-136">In Visual Web Developer there's a toolbar along the top showing various options available to you.</span></span> <span data-ttu-id="ca590-137">Istnieje również menu, które zapewnia inny sposób wykonywania zadań w środowisku IDE.</span><span class="sxs-lookup"><span data-stu-id="ca590-137">There's also a menu that provides another way to perform tasks in the IDE.</span></span> <span data-ttu-id="ca590-138">(Na przykład zamiast wybierania **nowego projektu** ze strony **początkowej** można użyć menu i wybrać polecenie **plik** &gt; **Nowy projekt**).</span><span class="sxs-lookup"><span data-stu-id="ca590-138">(For example, instead of selecting **New Project** from the **Start** page, you can use the menu and select **File** &gt; **New Project**.)</span></span>

[![](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="creating-your-first-application"></a><span data-ttu-id="ca590-139">Tworzenie pierwszej aplikacji</span><span class="sxs-lookup"><span data-stu-id="ca590-139">Creating Your First Application</span></span>

<span data-ttu-id="ca590-140">Aplikacje można tworzyć przy użyciu Visual Basic lub wizualizacji C# jako języka programowania.</span><span class="sxs-lookup"><span data-stu-id="ca590-140">You can create applications using either Visual Basic or Visual C# as the programming language.</span></span> <span data-ttu-id="ca590-141">Wybierz pozycję C# Wizualizacja po lewej stronie, a następnie wybierz pozycję **aplikacja sieci Web ASP.NET MVC 3**.</span><span class="sxs-lookup"><span data-stu-id="ca590-141">Select Visual C# on the left and then select **ASP.NET MVC 3 Web Application**.</span></span> <span data-ttu-id="ca590-142">Nadaj projektowi nazwę "MvcMovie", a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="ca590-142">Name your project "MvcMovie" and then click **OK**.</span></span> <span data-ttu-id="ca590-143">(Jeśli wolisz Visual Basic, przejdź do [Visual Basic wersji](../vb/intro-to-aspnet-mvc-3.md) tego samouczka).</span><span class="sxs-lookup"><span data-stu-id="ca590-143">(If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.)</span></span>

![](intro-to-aspnet-mvc-3/_static/image5.png)

<span data-ttu-id="ca590-144">W oknie dialogowym **Nowy projekt ASP.NET MVC 3** wybierz pozycję **aplikacja internetowa**.</span><span class="sxs-lookup"><span data-stu-id="ca590-144">In the **New ASP.NET MVC 3 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="ca590-145">Zaznacz opcję **Użyj znacznika HTML5** i pozostaw **Razor** jako domyślny aparat widoku.</span><span class="sxs-lookup"><span data-stu-id="ca590-145">Check **Use HTML5 markup** and leave **Razor** as the default view engine.</span></span>

![](intro-to-aspnet-mvc-3/_static/image6.png)

<span data-ttu-id="ca590-146">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="ca590-146">Click **OK**.</span></span> <span data-ttu-id="ca590-147">Visual Web Developer użył szablonu domyślnego dla projektu ASP.NET MVC, który właśnie został utworzony, więc masz działającą aplikację, która teraz nie wykonuje żadnych działań.</span><span class="sxs-lookup"><span data-stu-id="ca590-147">Visual Web Developer used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="ca590-148">To jest prosta "Hello world!"</span><span class="sxs-lookup"><span data-stu-id="ca590-148">This is a simple "Hello World!"</span></span> <span data-ttu-id="ca590-149">projekt i jest dobrym miejscem do uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ca590-149">project, and it's a good place to start your application.</span></span>

[![](intro-to-aspnet-mvc-3/_static/image8.png)](intro-to-aspnet-mvc-3/_static/image7.png)

<span data-ttu-id="ca590-150">Z menu **Debuguj** wybierz polecenie **Rozpocznij debugowanie**.</span><span class="sxs-lookup"><span data-stu-id="ca590-150">From the **Debug** menu, select **Start Debugging**.</span></span>

![](intro-to-aspnet-mvc-3/_static/image9.png)

<span data-ttu-id="ca590-151">Zwróć uwagę, że skrót klawiaturowy do rozpoczęcia debugowania jest F5.</span><span class="sxs-lookup"><span data-stu-id="ca590-151">Notice that the keyboard shortcut to start debugging is F5.</span></span>

<span data-ttu-id="ca590-152">F5 powoduje, że Visual Web Developer uruchamia programistyczny serwer sieci Web i uruchamia aplikację sieci Web.</span><span class="sxs-lookup"><span data-stu-id="ca590-152">F5 causes Visual Web Developer to start a development web server and run your web application.</span></span> <span data-ttu-id="ca590-153">Visual Web Developer następnie uruchamia przeglądarkę i otwiera stronę główną aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ca590-153">Visual Web Developer then launches a browser and opens the application's home page.</span></span> <span data-ttu-id="ca590-154">Zwróć uwagę, że na pasku adresu przeglądarki widnieją `localhost` a nie `example.com`.</span><span class="sxs-lookup"><span data-stu-id="ca590-154">Notice that the address bar of the browser says `localhost` and not something like `example.com`.</span></span> <span data-ttu-id="ca590-155">Jest to spowodowane tym, że `localhost` zawsze wskazuje na własny komputer lokalny, w tym przypadku uruchamia właśnie utworzoną aplikację.</span><span class="sxs-lookup"><span data-stu-id="ca590-155">That's because `localhost` always points to your own local computer, which in this case is running the application you just built.</span></span> <span data-ttu-id="ca590-156">Gdy Visual Web Developer uruchamia projekt sieci Web, dla serwera sieci Web jest używany port losowy.</span><span class="sxs-lookup"><span data-stu-id="ca590-156">When Visual Web Developer runs a web project, a random port is used for the web server.</span></span> <span data-ttu-id="ca590-157">Na poniższym obrazie losowy numer portu to 43246.</span><span class="sxs-lookup"><span data-stu-id="ca590-157">In the image below, the random port number is 43246.</span></span> <span data-ttu-id="ca590-158">Po uruchomieniu aplikacji prawdopodobnie zobaczysz inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="ca590-158">When you run the application, you'll probably see a different port number.</span></span>

![](intro-to-aspnet-mvc-3/_static/image10.png)

<span data-ttu-id="ca590-159">Z prawej strony pola ten szablon domyślny udostępnia dwie strony do odwiedzania oraz podstawową stronę logowania.</span><span class="sxs-lookup"><span data-stu-id="ca590-159">Right out of the box this default template gives you two pages to visit and a basic login page.</span></span> <span data-ttu-id="ca590-160">Następnym krokiem jest zmiana sposobu działania tej aplikacji i uzyskanie nieco informacji o ASP.NET MVC w procesie.</span><span class="sxs-lookup"><span data-stu-id="ca590-160">The next step is to change how this application works and learn a little bit about ASP.NET MVC in the process.</span></span> <span data-ttu-id="ca590-161">Zamknij przeglądarkę i Zmieńmy kod.</span><span class="sxs-lookup"><span data-stu-id="ca590-161">Close your browser and let's change some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ca590-162">Dalej</span><span class="sxs-lookup"><span data-stu-id="ca590-162">Next</span></span>](adding-a-controller.md)
