---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: Uzyskiwanie dostępu do danych modelu z kontrolera | Microsoft Docs
author: shanselman
description: Jest to samouczek początkującego, który wprowadza podstawy ASP.NET MVC. Utwórz prostą aplikację sieci Web, która odczytuje i zapisuje dane w bazie danych.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: 207ed880977d794d81efdc1ea458d17a68d501d8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78543751"
---
# <a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="cac63-104">Uzyskiwanie dostępu do danych modelu za pomocą kontrolera</span><span class="sxs-lookup"><span data-stu-id="cac63-104">Accessing your Model's Data from a Controller</span></span>

<span data-ttu-id="cac63-105">przez [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="cac63-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="cac63-106">Jest to samouczek początkującego, który wprowadza podstawy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="cac63-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="cac63-107">Utworzysz prostą aplikację sieci Web, która odczytuje i zapisuje dane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="cac63-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="cac63-108">Odwiedź [centrum learning ASP.NET MVC](../../../index.md) , aby znaleźć inne samouczki i przykłady MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="cac63-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>

<span data-ttu-id="cac63-109">W tej sekcji zamierzamy utworzyć nową klasę MoviesController i napisać kod, który pobiera nasze dane filmu i wyświetla go z powrotem do przeglądarki przy użyciu szablonu widoku.</span><span class="sxs-lookup"><span data-stu-id="cac63-109">In this section we are going to create a new MoviesController class, and write some code that retrieves our Movie data and displays it back to the browser using a View template.</span></span>

<span data-ttu-id="cac63-110">Kliknij prawym przyciskiem myszy folder controllers i Utwórz nowy MoviesController.</span><span class="sxs-lookup"><span data-stu-id="cac63-110">Right click on the Controllers folder and make a new MoviesController.</span></span>

<span data-ttu-id="cac63-111">[![dodać kontrolera](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="cac63-111">[![Add Controller](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span></span>

<span data-ttu-id="cac63-112">Spowoduje to utworzenie nowego pliku "MoviesController.cs" pod naszym folderem \Controllers w naszym projekcie.</span><span class="sxs-lookup"><span data-stu-id="cac63-112">This will create a new "MoviesController.cs" file underneath our \Controllers folder within our project.</span></span> <span data-ttu-id="cac63-113">Zaktualizujmy MovieController, aby pobrać listę filmów z ostatnio wypełnionej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="cac63-113">Let's update the MovieController to retrieve the list of movies from our newly populated database.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

<span data-ttu-id="cac63-114">Wykonujemy zapytanie LINQ, dzięki czemu pobieramy tylko filmy wydane po dniu 1984.</span><span class="sxs-lookup"><span data-stu-id="cac63-114">We are performing a LINQ query so that we only retrieve movies released after the summer of 1984.</span></span> <span data-ttu-id="cac63-115">Potrzebujemy szablonu widoku, aby renderować tę listę filmów z powrotem, więc kliknij prawym przyciskiem myszy metodę i wybierz polecenie Dodaj widok, aby je utworzyć.</span><span class="sxs-lookup"><span data-stu-id="cac63-115">We'll need a View template to render this list of movies back, so right-click in the method and select Add View to create it.</span></span>

<span data-ttu-id="cac63-116">W oknie dialogowym Dodawanie widoku zostanie wyświetlona lista&lt;filmów. models. Movie&gt; do naszego szablonu widoku.</span><span class="sxs-lookup"><span data-stu-id="cac63-116">Within the Add View dialog we'll indicate that we are passing a List&lt;Movies.Models.Movie&gt; to our View template.</span></span> <span data-ttu-id="cac63-117">W przeciwieństwie do poprzednich przypadków, w których użyto okna dialogowego Dodawanie widoku i wybrano opcję utworzenia szablonu "pusty", ten czas wskazuje, że program Visual Studio ma automatycznie "szkielet" jako szablon widoku dla nas z niestandardową zawartością.</span><span class="sxs-lookup"><span data-stu-id="cac63-117">Unlike the previous times we used the Add View dialog and chose to create an "Empty" template, this time we'll indicate that we want Visual Studio to automatically "scaffold" a view template for us with some default content.</span></span> <span data-ttu-id="cac63-118">W tym celu należy wybrać element "list" w menu rozwijanym "Wyświetl zawartość".</span><span class="sxs-lookup"><span data-stu-id="cac63-118">We'll do this by selecting the "List" item within the "View content dropdown menu.</span></span>

<span data-ttu-id="cac63-119">Należy pamiętać, że w przypadku utworzenia nowej klasy należy skompilować aplikację do wyświetlania w oknie dialogowym Dodawanie widoku.</span><span class="sxs-lookup"><span data-stu-id="cac63-119">Remember, when you have a created a new class you'll need to compile your application for it to show up in the Add View Dialog.</span></span>

![Dodaj widok](getting-started-with-mvc-part5/_static/image3.png)

<span data-ttu-id="cac63-121">Kliknij przycisk Dodaj, a system automatycznie wygeneruje kod dla tego widoku, który wyświetla naszą listę filmów.</span><span class="sxs-lookup"><span data-stu-id="cac63-121">Click add and the system will automatically generate the code for a View for us that displays our list of movies.</span></span> <span data-ttu-id="cac63-122">Jest to dobry moment, aby zmienić nagłówek &lt;H2&gt; na taki, jak "Moja lista filmów", tak jak wcześniej, za pomocą widoku Hello world.</span><span class="sxs-lookup"><span data-stu-id="cac63-122">This is a good time to change the &lt;h2&gt; heading to something like "My Movie List" like we did earlier with the Hello World view.</span></span>

<span data-ttu-id="cac63-123">[![filmów — Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="cac63-123">[![Movies - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span></span>

<span data-ttu-id="cac63-124">Uruchom aplikację i odwiedź stronę/Movies na pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="cac63-124">Run your application and visit /Movies in the address bar.</span></span> <span data-ttu-id="cac63-125">Teraz pobrano dane z bazy danych przy użyciu podstawowego zapytania wewnątrz kontrolera i zwróciło dane do widoku, który wie o filmach.</span><span class="sxs-lookup"><span data-stu-id="cac63-125">Now we've retrieved data from the database using a basic query inside the Controller and returned the data to a View that knows about Movies.</span></span> <span data-ttu-id="cac63-126">Ten widok umożliwia przechodzenie przez listę filmów i utworzenie tabeli danych dla nas.</span><span class="sxs-lookup"><span data-stu-id="cac63-126">That View then spins through the list of Movies and creates a table of data for us.</span></span>

<span data-ttu-id="cac63-127">[Lista filmów ![— Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="cac63-127">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span></span>

<span data-ttu-id="cac63-128">Nie będziemy wdrażać funkcji Edytuj, szczegóły i Usuń w tej aplikacji — dlatego nie są potrzebne domyślne linki utworzone dla nas przez szablon szkieletu.</span><span class="sxs-lookup"><span data-stu-id="cac63-128">We won't be implementing Edit, Details and Delete functionality with this application - so we don't need the default links that the scaffold template created for us.</span></span> <span data-ttu-id="cac63-129">Otwórz plik/Movies/Index.aspx i usuń go.</span><span class="sxs-lookup"><span data-stu-id="cac63-129">Open up the /Movies/Index.aspx file and remove them.</span></span>

<span data-ttu-id="cac63-130">Oto kod źródłowy, dla którego nasz zaktualizowany szablon widoku powinien wyglądać jak po wprowadzeniu tych zmian:</span><span class="sxs-lookup"><span data-stu-id="cac63-130">Here is the source code for what our updated View template should look like once we make these changes:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

<span data-ttu-id="cac63-131">Tworzy linki, które nie są potrzebne, więc usuniemy je z tego przykładu.</span><span class="sxs-lookup"><span data-stu-id="cac63-131">It's creating links that we won't need, so we'll delete them for this example.</span></span> <span data-ttu-id="cac63-132">Nasz nowy link zostanie zachowany w tym przypadku.</span><span class="sxs-lookup"><span data-stu-id="cac63-132">We will keep our Create New link though, as that's next!</span></span> <span data-ttu-id="cac63-133">Oto, jak nasza aplikacja zostanie usunięta z tą kolumną.</span><span class="sxs-lookup"><span data-stu-id="cac63-133">Here's what our app looks like with that column removed.</span></span>

<span data-ttu-id="cac63-134">[Lista filmów ![— Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="cac63-134">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span></span>

<span data-ttu-id="cac63-135">Mamy już prostą listę naszych danych filmowych.</span><span class="sxs-lookup"><span data-stu-id="cac63-135">We now have a simple listing of our movie data.</span></span> <span data-ttu-id="cac63-136">Jeśli jednak klikniemy link "Utwórz nowy", zostanie wyświetlony komunikat o błędzie, ponieważ nie jest on podłączany.</span><span class="sxs-lookup"><span data-stu-id="cac63-136">However, if we click the "Create New" link, we'll get an error as it's not hooked up!</span></span> <span data-ttu-id="cac63-137">Zaimplementujmy metodę tworzenia akcji i umożliwisz użytkownikowi wprowadzanie nowych filmów w naszej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="cac63-137">Let's implement a Create Action method and enable a user to enter new movies in our database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="cac63-138">[Poprzednie](getting-started-with-mvc-part4.md)
> [dalej](getting-started-with-mvc-part6.md)</span><span class="sxs-lookup"><span data-stu-id="cac63-138">[Previous](getting-started-with-mvc-part4.md)
[Next](getting-started-with-mvc-part6.md)</span></span>
