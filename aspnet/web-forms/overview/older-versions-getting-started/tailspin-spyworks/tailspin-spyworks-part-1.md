---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: 'Część 1: plik-> Nowy projekt | Microsoft Docs'
author: JoeStagner
description: Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji Tailspin Spyworks. Część 1 obejmuje przegląd i plik/nowy projekt.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: 05a3ace3d8fef9c1f3593f7948e42b4725d70134
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78636130"
---
# <a name="part-1-file--new-project"></a><span data-ttu-id="408cb-104">Część 1. Plik -> Nowy projekt</span><span class="sxs-lookup"><span data-stu-id="408cb-104">Part 1: File-> New Project</span></span>

<span data-ttu-id="408cb-105">Jan [Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="408cb-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="408cb-106">Tailspin Spyworks ilustruje, w jaki sposób bardzo jest prosta, aby tworzyć zaawansowane, skalowalne aplikacje dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="408cb-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="408cb-107">W tym artykule przedstawiono sposób korzystania z doskonałych nowych funkcji w programie ASP.NET 4 do tworzenia sklepu online, w tym zakupów, wyewidencjonowywania i administrowania.</span><span class="sxs-lookup"><span data-stu-id="408cb-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="408cb-108">Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="408cb-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="408cb-109">Część 1 obejmuje przegląd i plik/nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="408cb-109">Part 1 covers Overview and File/New Project.</span></span>

## <a id="_Toc260221666"></a><span data-ttu-id="408cb-110">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="408cb-110">Overview</span></span>

<span data-ttu-id="408cb-111">Ten samouczek stanowi wprowadzenie do ASP.NET formularzy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="408cb-111">This tutorial is an introduction to ASP.NET WebForms.</span></span> <span data-ttu-id="408cb-112">Zaczniemy powoli, więc środowisko programistyczne dla początkujących użytkowników jest w stanie dobra.</span><span class="sxs-lookup"><span data-stu-id="408cb-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="408cb-113">Aplikacja, którą będziemy kompilować, to prosty magazyn online.</span><span class="sxs-lookup"><span data-stu-id="408cb-113">The application we'll be building is a simple on-line store.</span></span>

![](tailspin-spyworks-part-1/_static/image1.jpg)

<span data-ttu-id="408cb-114">Goście mogą przeglądać produkty według kategorii:</span><span class="sxs-lookup"><span data-stu-id="408cb-114">Visitors can browse Products by Category:</span></span>

![](tailspin-spyworks-part-1/_static/image2.jpg)

<span data-ttu-id="408cb-115">Mogą oni wyświetlać jeden produkt i dodawać go do swojego koszyka:</span><span class="sxs-lookup"><span data-stu-id="408cb-115">They can view a single product and add it to their cart:</span></span>

![](tailspin-spyworks-part-1/_static/image3.jpg)

<span data-ttu-id="408cb-116">Mogą przeglądać ich koszyk, usuwając wszystkie elementy, które nie są już potrzebne:</span><span class="sxs-lookup"><span data-stu-id="408cb-116">They can review their cart, removing any items they no longer want:</span></span>

![](tailspin-spyworks-part-1/_static/image4.jpg)

<span data-ttu-id="408cb-117">Przeprowadzenie wyewidencjonowania spowoduje wyświetlenie monitu o</span><span class="sxs-lookup"><span data-stu-id="408cb-117">Proceeding to Checkout will prompt them to</span></span>

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

<span data-ttu-id="408cb-118">Po określeniu kolejności zobaczysz prosty ekran potwierdzenia:</span><span class="sxs-lookup"><span data-stu-id="408cb-118">After ordering, they see a simple confirmation screen:</span></span>

![](tailspin-spyworks-part-1/_static/image7.jpg)

<span data-ttu-id="408cb-119">Zaczniemy od utworzenia nowego projektu ASP.NET WebForms w programie Visual Studio 2010 i nastąpi przyrostowe dodanie funkcji w celu utworzenia kompletnej działającej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="408cb-119">We'll begin by creating a new ASP.NET WebForms project in Visual Studio 2010, and we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="408cb-120">W ten sposób będziemy obejmować dostęp do bazy danych, widoki list i siatki, strony aktualizacji danych, sprawdzanie poprawności danych przy użyciu stron wzorcowych na potrzeby spójnego układu strony, technologii AJAX, sprawdzania poprawności, członkostwa użytkowników i innych.</span><span class="sxs-lookup"><span data-stu-id="408cb-120">Along the way, we'll cover database access, list and grid views, data update pages, data validation, using master pages for consistent page layout, AJAX, validation, user membership, and more.</span></span>

<span data-ttu-id="408cb-121">Możesz wykonać instrukcje krok po kroku lub pobrać ukończoną aplikację z [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span><span class="sxs-lookup"><span data-stu-id="408cb-121">You can follow along step by step, or you can download the completed application from [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span></span>

<span data-ttu-id="408cb-122">Możesz użyć programu Visual Studio 2010 lub bezpłatnego programu Visual Web Developer 2010 z [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/).</span><span class="sxs-lookup"><span data-stu-id="408cb-122">You can use either Visual Studio 2010 or the free Visual Web Developer 2010 from [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="408cb-123">Aby skompilować aplikację, można użyć dowolnej SQL Server lub bezpłatnego SQL Server Express do hostowania bazy danych programu.</span><span class="sxs-lookup"><span data-stu-id="408cb-123">To build the application, you can use either SQL Server or the free SQL Server Express to host the database.</span></span>

## <a id="_Toc260221667"></a><span data-ttu-id="408cb-124">Plik/nowy projekt</span><span class="sxs-lookup"><span data-stu-id="408cb-124">File / New Project</span></span>

<span data-ttu-id="408cb-125">Zaczniemy od wybrania nowego projektu z menu plik w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="408cb-125">We'll start by selecting the New Project from the File menu in Visual Studio.</span></span> <span data-ttu-id="408cb-126">Spowoduje to wyświetlenie okna dialogowego Nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="408cb-126">This brings up the New Project dialog.</span></span>

![](tailspin-spyworks-part-1/_static/image8.jpg)

<span data-ttu-id="408cb-127">Wybierzemy grupę szablonów wizualizacji C# /sieci Web po lewej stronie, a następnie wybierz szablon "aplikacja sieci Web ASP.NET" w środkowej kolumnie.</span><span class="sxs-lookup"><span data-stu-id="408cb-127">We'll select the Visual C# / Web Templates group on the left, and then choose the "ASP.NET Web Application" template in the center column.</span></span> <span data-ttu-id="408cb-128">Nazwij projekt TailspinSpyworks i naciśnij przycisk OK.</span><span class="sxs-lookup"><span data-stu-id="408cb-128">Name your project TailspinSpyworks and press the OK button.</span></span>

![](tailspin-spyworks-part-1/_static/image9.jpg)

<span data-ttu-id="408cb-129">Spowoduje to utworzenie naszego projektu.</span><span class="sxs-lookup"><span data-stu-id="408cb-129">This will create our project.</span></span> <span data-ttu-id="408cb-130">Spójrzmy na foldery dołączone do naszej aplikacji w Eksplorator rozwiązań po prawej stronie.</span><span class="sxs-lookup"><span data-stu-id="408cb-130">Let's take a look at the folders that are included in our application in the Solution Explorer on the right side.</span></span>

![](tailspin-spyworks-part-1/_static/image10.jpg)

<span data-ttu-id="408cb-131">Puste rozwiązanie nie jest całkowicie puste — dodaje podstawową strukturę folderów:</span><span class="sxs-lookup"><span data-stu-id="408cb-131">The Empty Solution isn't completely empty – it adds a basic folder structure:</span></span>

![](tailspin-spyworks-part-1/_static/image1.png)

<span data-ttu-id="408cb-132">Należy zwrócić uwagę na konwencje zaimplementowane przez domyślny szablon projektu ASP.NET 4.</span><span class="sxs-lookup"><span data-stu-id="408cb-132">Note the conventions implemented by the ASP.NET 4 default project template.</span></span>

- <span data-ttu-id="408cb-133">Folder "Account" implementuje podstawowy interfejs użytkownika dla środowiska ASP. Podsystem członkostwa w sieci.</span><span class="sxs-lookup"><span data-stu-id="408cb-133">The "Account" folder implements a basic user interface for ASP.NET's membership subsystem.</span></span>
- <span data-ttu-id="408cb-134">Folder "Scripts" służy jako repozytorium dla plików JavaScript po stronie klienta, a podstawowe pliki jQuery. js są udostępniane domyślnie.</span><span class="sxs-lookup"><span data-stu-id="408cb-134">The "Scripts" folder serves as the repository for client side JavaScript files and the core jQuery .js files are made available by default.</span></span>
- <span data-ttu-id="408cb-135">Folder "Style" służy do organizowania wizualizacji witryn sieci Web (arkuszy stylów CSS)</span><span class="sxs-lookup"><span data-stu-id="408cb-135">The "Styles" folder is used to organize our web site visuals (CSS Style Sheets)</span></span>

<span data-ttu-id="408cb-136">Po naciśnięciu klawisza F5 w celu uruchomienia aplikacji i renderowania domyślnej strony. aspx zostanie wyświetlona następująca strona.</span><span class="sxs-lookup"><span data-stu-id="408cb-136">When we press F5 to run our application and render the default.aspx page we see the following.</span></span>

![](tailspin-spyworks-part-1/_static/image11.jpg)

<span data-ttu-id="408cb-137">Nasze pierwsze ulepszenie aplikacji będzie zastępować plik style. CSS z domyślnego szablonu WebForms z klasami CSS i skojarzonymi z nimi plikami obrazu, które będą asthetics wizualizację dla naszej aplikacji Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="408cb-137">Our first application enhancement will be to replace the Style.css file from the default WebForms template with the CSS classes and associated image files that will render the visual asthetics that we want for our Tailspin Spyworks application.</span></span>

<span data-ttu-id="408cb-138">Po wykonaniu tej operacji domyślna strona. aspx jest renderowana w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="408cb-138">After doing so our default.aspx page renders like this.</span></span>

![](tailspin-spyworks-part-1/_static/image12.jpg)

<span data-ttu-id="408cb-139">Zwróć uwagę na linki do obrazów w prawym górnym rogu strony i elementy menu, które zostały dodane do strony wzorcowej.</span><span class="sxs-lookup"><span data-stu-id="408cb-139">Notice the image links at the top right of the page and the menu items that have been added to the master page.</span></span> <span data-ttu-id="408cb-140">Tylko łącza "Zaloguj się" i "konto" wskazują strony istniejące (generowane przez szablon domyślny) oraz pozostałe strony, które zostaną wdrożone podczas kompilowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="408cb-140">Only the "Sign In" and "Account" links point to pages that exist (generated by the default template) and the rest of the pages we will implement as we build our application.</span></span>

<span data-ttu-id="408cb-141">Zamierzamy również przenieść stronę wzorcową do katalogu stylów.</span><span class="sxs-lookup"><span data-stu-id="408cb-141">We're also going to relocate the Master Page to the Styles directory.</span></span> <span data-ttu-id="408cb-142">Mimo że jest to tylko preferencja, może to trochę potrwać, jeśli chcemy, aby nasza aplikacja była "w przyszłości".</span><span class="sxs-lookup"><span data-stu-id="408cb-142">Though this is only a preference it may make things a little easier if we decide to make our application "skinable" in the future.</span></span>

<span data-ttu-id="408cb-143">Po wykonaniu tej czynności będziemy musieli zmienić odwołania do stron wzorcowych we wszystkich plikach aspx wygenerowanych przez domyślne strony formularzy sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="408cb-143">After doing this we'll need to change the master page references in all the .aspx files generated by the default ASP.NET WebForms pages.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="408cb-144">Dalej</span><span class="sxs-lookup"><span data-stu-id="408cb-144">Next</span></span>](tailspin-spyworks-part-2.md)
