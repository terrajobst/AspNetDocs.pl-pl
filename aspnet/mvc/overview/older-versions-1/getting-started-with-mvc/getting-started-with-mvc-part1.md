---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: Wprowadzenie do ASP.NET MVC | Microsoft Docs
author: shanselman
description: Jest to samouczek początkującego, który wprowadza podstawy ASP.NET MVC. Utwórz prostą aplikację sieci Web, która odczytuje i zapisuje dane w bazie danych.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: f8f0014776ba1313119e8c39c63a216b0fc864e7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78581586"
---
# <a name="intro-to-aspnet-mvc"></a><span data-ttu-id="331fe-104">Wprowadzenie do wzorca ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="331fe-104">Intro to ASP.NET MVC</span></span>

<span data-ttu-id="331fe-105">przez [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="331fe-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> > [!NOTE]
> > <span data-ttu-id="331fe-106">Zaktualizowana wersja, jeśli ten samouczek jest dostępny w [tym miejscu](../../getting-started/introduction/getting-started.md) przy użyciu [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013).</span><span class="sxs-lookup"><span data-stu-id="331fe-106">An updated version if this tutorial is available [here](../../getting-started/introduction/getting-started.md) using [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013).</span></span> <span data-ttu-id="331fe-107">W nowym samouczku jest używany ASP.NET MVC 5, który zapewnia wiele ulepszeń w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="331fe-107">The new tutorial uses ASP.NET MVC 5, which provides many improvements over this tutorial.</span></span>
>
>
> <span data-ttu-id="331fe-108">Jest to samouczek początkującego, który wprowadza podstawy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="331fe-108">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="331fe-109">Utworzysz prostą aplikację sieci Web, która odczytuje i zapisuje dane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="331fe-109">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="331fe-110">Odwiedź [centrum learning ASP.NET MVC](../../../index.md) , aby znaleźć inne samouczki i przykłady MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="331fe-110">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>

<span data-ttu-id="331fe-111">Utwórzmy pierwszą aplikację sieci Web ASP.NET MVC przy użyciu [programu Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/).</span><span class="sxs-lookup"><span data-stu-id="331fe-111">Let's make our first ASP.NET MVC Web Application using [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="331fe-112">Udostępnimy małą listę filmów, która pozwoli nam tworzyć i wyświetlać filmy.</span><span class="sxs-lookup"><span data-stu-id="331fe-112">We'll make a little Movie list application that will let us create and list movies.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="331fe-113">Co będziesz kompilować</span><span class="sxs-lookup"><span data-stu-id="331fe-113">What You'll Build</span></span>

<span data-ttu-id="331fe-114">Poniżej przedstawiono dwa zrzuty ekranu aplikacji, która zostanie utworzona.</span><span class="sxs-lookup"><span data-stu-id="331fe-114">Here are two screenshots of the application you'll build.</span></span> <span data-ttu-id="331fe-115">Będziesz mieć prostą tabelę filmów z różnymi kolumnami.</span><span class="sxs-lookup"><span data-stu-id="331fe-115">You will have a simple table of movies with various columns.</span></span>

<span data-ttu-id="331fe-116">[Lista filmów ![— Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="331fe-116">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)</span></span>

<span data-ttu-id="331fe-117">A będziesz mieć formularz tworzenia, aby umożliwić Dodawanie filmów do listy.</span><span class="sxs-lookup"><span data-stu-id="331fe-117">And you'll have a Create Form so we can add movies to the list.</span></span>

<span data-ttu-id="331fe-118">[![utworzyć filmu — Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="331fe-118">[![Create a Movie - Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)</span></span>

## <a name="skills-youll-learn"></a><span data-ttu-id="331fe-119">Posiadane umiejętności</span><span class="sxs-lookup"><span data-stu-id="331fe-119">Skills You'll Learn</span></span>

<span data-ttu-id="331fe-120">Ten samouczek zawiera informacje na temat tworzenia aplikacji sieci Web ASP.NET MVC przy użyciu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="331fe-120">This tutorial will teach you the basics of building an ASP.NET MVC Web Application using Visual Studio.</span></span> <span data-ttu-id="331fe-121">W dokumencie omówiono następujące zagadnienia :</span><span class="sxs-lookup"><span data-stu-id="331fe-121">You'll learn:</span></span>

- <span data-ttu-id="331fe-122">Jak utworzyć nowy projekt ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="331fe-122">How to create a new ASP.NET MVC Project</span></span>
- <span data-ttu-id="331fe-123">Jak utworzyć nową bazę danych za pomocą SQL Server</span><span class="sxs-lookup"><span data-stu-id="331fe-123">How to create a new Database with SQL Server</span></span>
- <span data-ttu-id="331fe-124">Jak utworzyć kontrolery i widoki ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="331fe-124">How to create ASP.NET MVC Controllers and Views</span></span>
- <span data-ttu-id="331fe-125">Jak pobierać i wyświetlać dane</span><span class="sxs-lookup"><span data-stu-id="331fe-125">How to retrieve and display data</span></span>
- <span data-ttu-id="331fe-126">Jak edytować dane i włączać sprawdzanie poprawności danych</span><span class="sxs-lookup"><span data-stu-id="331fe-126">How to edit data and enable data validation</span></span>
- <span data-ttu-id="331fe-127">Jak zaktualizować schemat bazy danych</span><span class="sxs-lookup"><span data-stu-id="331fe-127">How to update the database schema</span></span>

## <a name="get-started"></a><span data-ttu-id="331fe-128">Rozpoczęcie pracy</span><span class="sxs-lookup"><span data-stu-id="331fe-128">Get Started</span></span>

<span data-ttu-id="331fe-129">Zacznij od uruchomienia programu Visual Web Developer 2010 Express (wywołam teraz "pliku VWD") i wybierz pozycję Nowy projekt na ekranie startowym.</span><span class="sxs-lookup"><span data-stu-id="331fe-129">Start by running Visual Web Developer 2010 Express (I'll call it "VWD" from now on) and select New Project from the Start Screen.</span></span>

<span data-ttu-id="331fe-130">Visual Web Developer to IDE lub zintegrowane środowisko deweloperskie.</span><span class="sxs-lookup"><span data-stu-id="331fe-130">Visual Web Developer is an IDE, or Integrated Developer Environment.</span></span> <span data-ttu-id="331fe-131">Podobnie jak w przypadku używania programu Microsoft Word do pisania dokumentów, do tworzenia aplikacji będziesz używać środowiska IDE.</span><span class="sxs-lookup"><span data-stu-id="331fe-131">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="331fe-132">Na początku znajduje się pasek narzędzi pokazujący różne dostępne opcje, a także menu, które można także użyć do wybrania pliku | Nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="331fe-132">There's a toolbar along the top showing various options available to you, as well as the menu you could also have used to Select File | New project.</span></span>

<span data-ttu-id="331fe-133">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="331fe-133">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)</span></span>

## <a name="creating-your-first-application"></a><span data-ttu-id="331fe-134">Tworzenie pierwszej aplikacji</span><span class="sxs-lookup"><span data-stu-id="331fe-134">Creating Your First Application</span></span>

<span data-ttu-id="331fe-135">Aplikacje można tworzyć przy użyciu Visual Basic lub wizualizacji C#.</span><span class="sxs-lookup"><span data-stu-id="331fe-135">You can create applications using Visual Basic or Visual C#.</span></span> <span data-ttu-id="331fe-136">Na razie zaznacz opcję Visual C# po lewej stronie, a następnie wybierz pozycję "ASP.NET MVC 2 Web Application".</span><span class="sxs-lookup"><span data-stu-id="331fe-136">For now, Select Visual C# on the left, then pick "ASP.NET MVC 2 Web Application."</span></span> <span data-ttu-id="331fe-137">Nadaj projektowi nazwę "filmy" i kliknij przycisk OK.</span><span class="sxs-lookup"><span data-stu-id="331fe-137">Name your project "Movies" and click OK.</span></span>

<span data-ttu-id="331fe-138">[![nowy projekt](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="331fe-138">[![New Project](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)</span></span>

<span data-ttu-id="331fe-139">Po prawej stronie jest Eksplorator rozwiązań pokazujący wszystkie pliki i foldery w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="331fe-139">On the right-hand side is the Solution Explorer showing all the files and folders in your application.</span></span> <span data-ttu-id="331fe-140">Duże okno w środku polega na tym, że edytujesz swój kod i spędzasz najwięcej czasu.</span><span class="sxs-lookup"><span data-stu-id="331fe-140">The big window in the middle is where you edit your code and spend most of your time.</span></span> <span data-ttu-id="331fe-141">Program Visual Studio użył szablonu domyślnego dla projektu ASP.NET MVC, który właśnie został utworzony, więc masz działającą aplikację, która teraz nie wykonuje żadnych działań.</span><span class="sxs-lookup"><span data-stu-id="331fe-141">Visual Studio used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="331fe-142">To jest prosta "Hello world!</span><span class="sxs-lookup"><span data-stu-id="331fe-142">This is a simple "Hello World!</span></span> <span data-ttu-id="331fe-143">projekt i jest dobrym miejscem do rozpoczęcia pracy z naszą aplikacją.</span><span class="sxs-lookup"><span data-stu-id="331fe-143">project, and it is a good place to start for our application.</span></span>

<span data-ttu-id="331fe-144">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="331fe-144">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)</span></span>

<span data-ttu-id="331fe-145">Wybierz przycisk "Odtwórz" na pasku narzędzi.</span><span class="sxs-lookup"><span data-stu-id="331fe-145">Select the "play" button to the toolbar.</span></span>

![Rozpocznij debugowanie](getting-started-with-mvc-part1/_static/image11.png)

<span data-ttu-id="331fe-147">Jest to zielona strzałka wskazująca prawo do skompilowania programu i uruchomienia aplikacji w przeglądarce sieci Web.</span><span class="sxs-lookup"><span data-stu-id="331fe-147">It's a green arrow pointing to the right that will compile your program and start your application in a web browser.</span></span>

<span data-ttu-id="331fe-148">*Uwaga: możesz zamiast tego nacisnąć klawisz F5 na klawiaturze lub wybrać polecenie Debuguj-&gt;Rozpocznij debugowanie z menu "Debugowanie".*</span><span class="sxs-lookup"><span data-stu-id="331fe-148">*NOTE: You can instead press F5 on your keyboard, or select Debug-&gt;Start Debugging from the "Debug" menu.*</span></span>

<span data-ttu-id="331fe-149">Spowoduje to, że Visual Web Developer rozpocznie Programowanie na serwerze sieci Web i uruchamianie naszej aplikacji sieci Web (nie ma konieczności konfigurowania ani ręcznego wykonywania czynności).</span><span class="sxs-lookup"><span data-stu-id="331fe-149">This will cause Visual Web Developer to start a development web-server and run our web application (there are no configuration or manual steps required to enable this).</span></span> <span data-ttu-id="331fe-150">Następnie zostanie uruchomiona przeglądarka i zostanie skonfigurowana w celu przeglądania strony głównej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="331fe-150">It will then launch a browser and configure it to browse the application's home-page.</span></span> <span data-ttu-id="331fe-151">Zwróć uwagę, że na pasku adresu przeglądarki jest wyświetlany napis "localhost", a nie coś, takiego jak example.com.</span><span class="sxs-lookup"><span data-stu-id="331fe-151">Notice below that the address bar of the browser says "localhost", and not something like example.com.</span></span> <span data-ttu-id="331fe-152">Jest to spowodowane tym, że hosty localhost zawsze wskazuje na własny komputer lokalny — w tym przypadku jest uruchomiona utworzona przez nas aplikacja.</span><span class="sxs-lookup"><span data-stu-id="331fe-152">That's because localhost always points to your own local computer - which in this case is running the application we just built.</span></span>

<span data-ttu-id="331fe-153">[Strona główna ![](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="331fe-153">[![Home Page](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)</span></span>

<span data-ttu-id="331fe-154">Z pola ten szablon domyślny udostępnia dwie strony do odwiedzania oraz podstawową stronę logowania.</span><span class="sxs-lookup"><span data-stu-id="331fe-154">Out of the box this default template gives you two pages to visit and a basic login page.</span></span> <span data-ttu-id="331fe-155">Zmieńmy sposób działania tej aplikacji i Dowiedz się więcej na temat ASP.NET MVC w procesie.</span><span class="sxs-lookup"><span data-stu-id="331fe-155">Let's change how this application works and learn a little bit about ASP.NET MVC in the process.</span></span> <span data-ttu-id="331fe-156">Zamknij przeglądarkę i umożliwia zmianę kodu.</span><span class="sxs-lookup"><span data-stu-id="331fe-156">Close your browser and lets change some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="331fe-157">Dalej</span><span class="sxs-lookup"><span data-stu-id="331fe-157">Next</span></span>](getting-started-with-mvc-part2.md)
