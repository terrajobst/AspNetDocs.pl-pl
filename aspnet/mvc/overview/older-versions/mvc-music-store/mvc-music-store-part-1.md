---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: 'Część 1: przegląd i plik > Nowy projekt | Microsoft Docs'
author: jongalloway
description: Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji do sklepu ASP.NET MVC Music. Część 1 obejmuje przegląd i plik > Nowy projekt.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: 48428ff4ab5888253ed93ac41e79006eec823ad2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559984"
---
# <a name="part-1-overview-and-file-new-project"></a><span data-ttu-id="25e18-104">Część 1. Omówienie i Plik->Nowy projekt</span><span class="sxs-lookup"><span data-stu-id="25e18-104">Part 1: Overview and File->New Project</span></span>

<span data-ttu-id="25e18-105">przez [Jan Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="25e18-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="25e18-106">Sklep MVC Music jest aplikacją samouczka, która wprowadza i objaśnia krok po kroku, jak używać ASP.NET MVC i Visual Studio do programowania w sieci Web.</span><span class="sxs-lookup"><span data-stu-id="25e18-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="25e18-107">Sklep MVC Music jest lekkim przykładowym wdrożeniem magazynu, który sprzedaje Albumy muzyczne w trybie online i implementuje podstawowe funkcje administracyjne, logowania użytkownika i koszyka.</span><span class="sxs-lookup"><span data-stu-id="25e18-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="25e18-108">Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji do sklepu ASP.NET MVC Music.</span><span class="sxs-lookup"><span data-stu-id="25e18-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="25e18-109">Część 1 obejmuje przegląd i plik&gt;nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="25e18-109">Part 1 covers Overview and File-&gt;New Project.</span></span>

## <a name="overview"></a><span data-ttu-id="25e18-110">Omówienie</span><span class="sxs-lookup"><span data-stu-id="25e18-110">Overview</span></span>

<span data-ttu-id="25e18-111">Sklep MVC Music jest aplikacją samouczka, która wprowadza i objaśnia krok po kroku, jak używać ASP.NET MVC i Visual Web Developer for Web Development.</span><span class="sxs-lookup"><span data-stu-id="25e18-111">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Web Developer for web development.</span></span> <span data-ttu-id="25e18-112">Zaczniemy powoli, więc środowisko programistyczne dla początkujących użytkowników jest w stanie dobra.</span><span class="sxs-lookup"><span data-stu-id="25e18-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="25e18-113">Aplikacja, którą będziemy kompilować, to prosty magazyn utworów muzycznych.</span><span class="sxs-lookup"><span data-stu-id="25e18-113">The application we'll be building is a simple music store.</span></span> <span data-ttu-id="25e18-114">Istnieją trzy główne części aplikacji: zakupy, Wyewidencjonowywanie i administrowanie.</span><span class="sxs-lookup"><span data-stu-id="25e18-114">There are three main parts to the application: shopping, checkout, and administration.</span></span>

![](mvc-music-store-part-1/_static/image1.jpg)

<span data-ttu-id="25e18-115">Goście mogą przeglądać albumy według gatunku:</span><span class="sxs-lookup"><span data-stu-id="25e18-115">Visitors can browse Albums by Genre:</span></span>

![](mvc-music-store-part-1/_static/image2.jpg)

<span data-ttu-id="25e18-116">Mogą oni wyświetlać jeden album i dodawać go do swojego koszyka:</span><span class="sxs-lookup"><span data-stu-id="25e18-116">They can view a single album and add it to their cart:</span></span>

![](mvc-music-store-part-1/_static/image3.jpg)

<span data-ttu-id="25e18-117">Mogą przeglądać ich koszyk, usuwając wszystkie elementy, które nie są już potrzebne:</span><span class="sxs-lookup"><span data-stu-id="25e18-117">They can review their cart, removing any items they no longer want:</span></span>

![](mvc-music-store-part-1/_static/image4.jpg)

<span data-ttu-id="25e18-118">W wyniku wyewidencjonowania zostanie wyświetlony monit o zalogowanie lub zarejestrowanie konta użytkownika.</span><span class="sxs-lookup"><span data-stu-id="25e18-118">Proceeding to Checkout will prompt them to login or register for a user account.</span></span>

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

<span data-ttu-id="25e18-119">Po utworzeniu konta można wykonać zamówienie, wypełniając informacje o wysyłce i płatności.</span><span class="sxs-lookup"><span data-stu-id="25e18-119">After creating an account, they can complete the order by filling out shipping and payment information.</span></span> <span data-ttu-id="25e18-120">Aby zachować prostotę, korzystamy z wspaniałej promocji: wszystko jest bezpłatne, jeśli wprowadzisz kod promocyjny "bezpłatnie"!</span><span class="sxs-lookup"><span data-stu-id="25e18-120">To keep things simple, we're running an amazing promotion: everything's free if they enter promotion code "FREE"!</span></span>

![](mvc-music-store-part-1/_static/image5.jpg)

<span data-ttu-id="25e18-121">Po określeniu kolejności zobaczysz prosty ekran potwierdzenia:</span><span class="sxs-lookup"><span data-stu-id="25e18-121">After ordering, they see a simple confirmation screen:</span></span>

![](mvc-music-store-part-1/_static/image6.jpg)

<span data-ttu-id="25e18-122">Oprócz stron przeznaczonych dla klientów zostanie również utworzona sekcja administrator, która zawiera listę albumów, z których administratorzy mogą tworzyć, edytować i usuwać albumy:</span><span class="sxs-lookup"><span data-stu-id="25e18-122">In addition to customer-facing pages, we'll also build an administrator section that shows a list of albums from which Administrators can Create, Edit, and Delete albums:</span></span>

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a><span data-ttu-id="25e18-123">1. plik —&gt; nowy projekt</span><span class="sxs-lookup"><span data-stu-id="25e18-123">1. File -&gt; New Project</span></span>

### <a name="installing-the-software"></a><span data-ttu-id="25e18-124">Instalowanie oprogramowania</span><span class="sxs-lookup"><span data-stu-id="25e18-124">Installing the software</span></span>

<span data-ttu-id="25e18-125">Ten samouczek rozpoczyna się od utworzenia nowego projektu ASP.NET MVC 3 przy użyciu bezpłatnego programu Visual Web Developer 2010 Express (który jest bezpłatny), a następnie zwiększy możliwości, aby utworzyć kompletną, działającą aplikację.</span><span class="sxs-lookup"><span data-stu-id="25e18-125">This tutorial will begin by creating a new ASP.NET MVC 3 project using the free Visual Web Developer 2010 Express (which is free), and then we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="25e18-126">W ten sposób będziemy obejmować dostęp do bazy danych, scenariusze publikowania formularzy i sprawdzanie poprawności danych przy użyciu stron wzorcowych dla spójnego układu strony przy użyciu technologii AJAX do aktualizacji stron i weryfikacji, logowania użytkownika i innych.</span><span class="sxs-lookup"><span data-stu-id="25e18-126">Along the way, we'll cover database access, form posting scenarios, data validation, using master pages for consistent page layout, using AJAX for page updates and validation, user login, and more.</span></span>

<span data-ttu-id="25e18-127">Możesz wykonać instrukcje krok po kroku lub pobrać ukończoną aplikację ze [sklepu MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="25e18-127">You can follow along step by step, or you can download the completed application from [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="25e18-128">Do skompilowania aplikacji można użyć programu Visual Studio 2010 z dodatkiem SP1 lub Visual Web Developer 2010 Express SP1 (bezpłatna wersja programu Visual Studio 2010).</span><span class="sxs-lookup"><span data-stu-id="25e18-128">You can use either Visual Studio 2010 SP1 or Visual Web Developer 2010 Express SP1 (a free version of Visual Studio 2010) to build the application.</span></span> <span data-ttu-id="25e18-129">Do hostowania bazy danych będziemy używać SQL Server Compact (również wolnych).</span><span class="sxs-lookup"><span data-stu-id="25e18-129">We'll be using the SQL Server Compact (also free) to host the database.</span></span> <span data-ttu-id="25e18-130">Przed rozpoczęciem upewnij się, że zainstalowano wymagania wstępne wymienione poniżej.</span><span class="sxs-lookup"><span data-stu-id="25e18-130">Before you start, make sure you've installed the prerequisites listed below.</span></span>

- <span data-ttu-id="25e18-131">[Wymagania wstępne programu Visual Studio Web Developer Express SP1]</span><span class="sxs-lookup"><span data-stu-id="25e18-131">[Visual Studio Web Developer Express SP1 prerequisites]</span></span>
- <span data-ttu-id="25e18-132">[ASP.NET MVC 3 Tools Update]</span><span class="sxs-lookup"><span data-stu-id="25e18-132">[ASP.NET MVC 3 Tools Update]</span></span>
- <span data-ttu-id="25e18-133">[SQL Server Compact 4,0] — w tym obsługa środowiska uruchomieniowego i narzędzi</span><span class="sxs-lookup"><span data-stu-id="25e18-133">[SQL Server Compact 4.0] - including both runtime and tools support</span></span>

### <a name="creating-a-new-aspnet-mvc-3-project"></a><span data-ttu-id="25e18-134">Tworzenie nowego projektu ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="25e18-134">Creating a new ASP.NET MVC 3 project</span></span>

<span data-ttu-id="25e18-135">Zaczniemy od wybrania pozycji "nowy projekt" z menu plik w programie Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="25e18-135">We'll start by selecting "New Project" from the File menu in Visual Web Developer.</span></span> <span data-ttu-id="25e18-136">Spowoduje to wyświetlenie okna dialogowego Nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="25e18-136">This brings up the New Project dialog.</span></span>

![](mvc-music-store-part-1/_static/image5.png)

<span data-ttu-id="25e18-137">Po lewej stronie wybierzesz C# grupę szablonów sieci Web dla wizualizacji&gt;, a następnie wybierz szablon "aplikacja sieci Web ASP.NET MVC 3" w środkowej kolumnie.</span><span class="sxs-lookup"><span data-stu-id="25e18-137">We'll select the Visual C# -&gt; Web Templates group on the left, then choose the "ASP.NET MVC 3 Web Application" template in the center column.</span></span> <span data-ttu-id="25e18-138">Nazwij projekt MvcMusicStore i naciśnij przycisk OK.</span><span class="sxs-lookup"><span data-stu-id="25e18-138">Name your project MvcMusicStore and press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image8.jpg)

<span data-ttu-id="25e18-139">Spowoduje to wyświetlenie pomocniczego okna dialogowego, które umożliwi nam wprowadzanie określonych ustawień MVC dla projektu.</span><span class="sxs-lookup"><span data-stu-id="25e18-139">This will display a secondary dialog which allows us to make some MVC specific settings for our project.</span></span> <span data-ttu-id="25e18-140">Wybierz następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="25e18-140">Select the following:</span></span>

<span data-ttu-id="25e18-141">Szablon projektu — wybierz opcję pusty</span><span class="sxs-lookup"><span data-stu-id="25e18-141">Project Template - select Empty</span></span>

<span data-ttu-id="25e18-142">Wyświetl aparat — wybierz Razor</span><span class="sxs-lookup"><span data-stu-id="25e18-142">View Engine - select Razor</span></span>

<span data-ttu-id="25e18-143">Użyj znacznika semantyki HTML5 — zaznaczone</span><span class="sxs-lookup"><span data-stu-id="25e18-143">Use HTML5 semantic markup - checked</span></span>

<span data-ttu-id="25e18-144">Sprawdź, czy ustawienia są następujące, jak pokazano poniżej, a następnie naciśnij przycisk OK.</span><span class="sxs-lookup"><span data-stu-id="25e18-144">Verify that your settings are as shown below, then press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image9.jpg)

<span data-ttu-id="25e18-145">Spowoduje to utworzenie naszego projektu.</span><span class="sxs-lookup"><span data-stu-id="25e18-145">This will create our project.</span></span> <span data-ttu-id="25e18-146">Zapoznaj się z folderami, które zostały dodane do naszej aplikacji w Eksplorator rozwiązań po prawej stronie.</span><span class="sxs-lookup"><span data-stu-id="25e18-146">Let's take a look at the folders that have been added to our application in the Solution Explorer on the right side.</span></span>

![](mvc-music-store-part-1/_static/image10.jpg)

<span data-ttu-id="25e18-147">Pusty szablon MVC 3 nie jest całkowicie pusty — dodaje podstawową strukturę folderów:</span><span class="sxs-lookup"><span data-stu-id="25e18-147">The Empty MVC 3 template isn't completely empty – it adds a basic folder structure:</span></span>

![](mvc-music-store-part-1/_static/image6.png)

<span data-ttu-id="25e18-148">ASP.NET MVC wykorzystuje podstawowe konwencje nazewnictwa dla nazw folderów:</span><span class="sxs-lookup"><span data-stu-id="25e18-148">ASP.NET MVC makes use of some basic naming conventions for folder names:</span></span>

| <span data-ttu-id="25e18-149">**Folder**</span><span class="sxs-lookup"><span data-stu-id="25e18-149">**Folder**</span></span> | <span data-ttu-id="25e18-150">**Przeznaczenie**</span><span class="sxs-lookup"><span data-stu-id="25e18-150">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="25e18-151">**/Controllers**</span><span class="sxs-lookup"><span data-stu-id="25e18-151">**/Controllers**</span></span> | <span data-ttu-id="25e18-152">Kontrolery odpowiadają na dane wejściowe z przeglądarki, decydują o tym, co należy zrobić z nim, i zwracają odpowiedź do użytkownika.</span><span class="sxs-lookup"><span data-stu-id="25e18-152">Controllers respond to input from the browser, decide what to do with it, and return response to the user.</span></span> |
| <span data-ttu-id="25e18-153">**/Views**</span><span class="sxs-lookup"><span data-stu-id="25e18-153">**/Views**</span></span> | <span data-ttu-id="25e18-154">Widoki przechowują szablony interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="25e18-154">Views hold our UI templates</span></span> |
| <span data-ttu-id="25e18-155">**/Models**</span><span class="sxs-lookup"><span data-stu-id="25e18-155">**/Models**</span></span> | <span data-ttu-id="25e18-156">Modele przechowują dane i manipulowanie nimi</span><span class="sxs-lookup"><span data-stu-id="25e18-156">Models hold and manipulate data</span></span> |
| <span data-ttu-id="25e18-157">**/Content**</span><span class="sxs-lookup"><span data-stu-id="25e18-157">**/Content**</span></span> | <span data-ttu-id="25e18-158">Ten folder zawiera nasze obrazy, CSS i wszelkie inne zawartość statyczną</span><span class="sxs-lookup"><span data-stu-id="25e18-158">This folder holds our images, CSS, and any other static content</span></span> |
| <span data-ttu-id="25e18-159">**/Scripts**</span><span class="sxs-lookup"><span data-stu-id="25e18-159">**/Scripts**</span></span> | <span data-ttu-id="25e18-160">Ten folder zawiera nasze pliki JavaScript</span><span class="sxs-lookup"><span data-stu-id="25e18-160">This folder holds our JavaScript files</span></span> |

<span data-ttu-id="25e18-161">Te foldery są uwzględniane nawet w pustej aplikacji ASP.NET MVC, ponieważ struktura ASP.NET MVC domyślnie używa podejścia "Konwencja od konfiguracji" i wprowadza pewne domyślne założenia na podstawie konwencji nazewnictwa folderów.</span><span class="sxs-lookup"><span data-stu-id="25e18-161">These folders are included even in an Empty ASP.NET MVC application because the ASP.NET MVC framework by default uses a "convention over configuration" approach and makes some default assumptions based on folder naming conventions.</span></span> <span data-ttu-id="25e18-162">Na przykład kontrolery szukają widoków w folderze widoki domyślnie, bez konieczności jawnego określania tego w kodzie.</span><span class="sxs-lookup"><span data-stu-id="25e18-162">For instance, controllers look for views in the Views folder by default without you having to explicitly specify this in your code.</span></span> <span data-ttu-id="25e18-163">Naklejanie z konwencjami domyślnymi zmniejsza ilość kodu, który trzeba napisać, i ułatwia innym deweloperom zrozumienie Twojego projektu.</span><span class="sxs-lookup"><span data-stu-id="25e18-163">Sticking with the default conventions reduces the amount of code you need to write, and can also make it easier for other developers to understand your project.</span></span> <span data-ttu-id="25e18-164">Wyjaśnimy te konwencje bardziej jak w przypadku kompilowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="25e18-164">We'll explain these conventions more as we build our application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="25e18-165">Dalej</span><span class="sxs-lookup"><span data-stu-id="25e18-165">Next</span></span>](mvc-music-store-part-2.md)
