---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: ASP.NET MVC 4 — podstawy | Microsoft Docs
author: rick-anderson
description: To praktyczne laboratorium jest oparte na sklepie MVC (Model View Controller), aplikacji samouczka, która wprowadza i objaśnia krok po kroku, jak używać ASP.NET MV...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: 95e9b9f55b2080c0ed01dc34e3a32f9f1c905644
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598820"
---
# <a name="aspnet-mvc-4-fundamentals"></a><span data-ttu-id="4ed0b-103">ASP.NET MVC 4 — podstawy</span><span class="sxs-lookup"><span data-stu-id="4ed0b-103">ASP.NET MVC 4 Fundamentals</span></span>

<span data-ttu-id="4ed0b-104">przez [zespół Camp sieci Web](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="4ed0b-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="4ed0b-105">Pobierz zestaw szkoleniowy dla sieci Web Camp</span><span class="sxs-lookup"><span data-stu-id="4ed0b-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="4ed0b-106">To praktyczne laboratorium jest oparte na sklepie MVC (Model View Controller) — aplikacji samouczka, która wprowadza i objaśnia krok po kroku, jak używać ASP.NET MVC i programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-106">This Hands-On Lab is based on MVC (Model View Controller) Music Store, a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio.</span></span> <span data-ttu-id="4ed0b-107">W całym laboratorium uczysz się prostoty, a tym samym korzystać z tych technologii jednocześnie.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-107">Throughout the lab you will learn the simplicity, yet power of using these technologies together.</span></span> <span data-ttu-id="4ed0b-108">Zaczniesz korzystać z prostej aplikacji i utworzymy ją do momentu, gdy będzie ona w pełni funkcjonalna ASP.NET aplikacji sieci Web MVC 4.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-108">You will start with a simple application and will build it until you have a fully functional ASP.NET MVC 4 Web Application.</span></span>

<span data-ttu-id="4ed0b-109">To laboratorium działa z ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-109">This Lab works with ASP.NET MVC 4.</span></span>

<span data-ttu-id="4ed0b-110">Jeśli chcesz poznać wersję ASP.NET MVC 3 aplikacji samouczka, możesz ją znaleźć w [sklepie MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="4ed0b-110">If you wish to explore the ASP.NET MVC 3 version of the tutorial application, you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="4ed0b-111">W ramach tego praktycznego laboratorium przyjęto założenie, że deweloper ma doświadczenie w technologiach programistycznych dla sieci Web, takich jak HTML i JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-111">This Hands-On Lab assumes that the developer has experience in Web development technologies, such as HTML and JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="4ed0b-112">Wszystkie przykładowe kod i fragmenty kodu są zawarte w zestawie szkoleń w sieci Web Camp, które są dostępne w [wersjach Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="4ed0b-112">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="4ed0b-113">Projekt charakterystyczny dla tego laboratorium jest dostępny pod adresem [ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).</span><span class="sxs-lookup"><span data-stu-id="4ed0b-113">The project specific to this lab is available at [ASP.NET MVC 4 Fundamentals](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).</span></span>

<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a><span data-ttu-id="4ed0b-114">Aplikacja ze sklepu muzycznego</span><span class="sxs-lookup"><span data-stu-id="4ed0b-114">The Music Store application</span></span>

<span data-ttu-id="4ed0b-115">Aplikacja sieci Web do przechowywania utworów muzycznych, która będzie skompilowana w ramach tego laboratorium, obejmuje trzy główne części: zakupy, Wyewidencjonowywanie i administrowanie.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-115">The Music Store web application that will be built throughout this Lab comprises three main parts: shopping, checkout, and administration.</span></span> <span data-ttu-id="4ed0b-116">Osoby odwiedzające będą mogły przeglądać albumy według gatunku, dodawać albumy do swojego koszyka, sprawdzać ich wybór, a następnie przejść do wyewidencjonowania, aby zalogować się i zakończyć zamówienie.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-116">Visitors will be able to browse albums by genre, add albums to their cart, review their selection and finally proceed to checkout to login and complete the order.</span></span> <span data-ttu-id="4ed0b-117">Ponadto administratorzy sklepu będą mogli zarządzać dostępnymi albumami oraz ich głównymi właściwościami.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-117">Additionally, store administrators will be able to manage the available albums as well as their main properties.</span></span>

<span data-ttu-id="4ed0b-118">![Ekrany sklepu muzycznego](aspnet-mvc-4-fundamentals/_static/image1.png "Ekrany sklepu muzycznego")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-118">![Music Store screens](aspnet-mvc-4-fundamentals/_static/image1.png "Music Store screens")</span></span>

<span data-ttu-id="4ed0b-119">*Ekrany sklepu muzycznego*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-119">*Music Store screens*</span></span>

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a><span data-ttu-id="4ed0b-120">ASP.NET MVC 4 Essentials</span><span class="sxs-lookup"><span data-stu-id="4ed0b-120">ASP.NET MVC 4 Essentials</span></span>

<span data-ttu-id="4ed0b-121">Aplikacja ze sklepu muzycznego będzie skompilowana przy użyciu **kontrolera widoku modelu (MVC)** , wzorca architektury, który oddziela aplikację do trzech głównych składników:</span><span class="sxs-lookup"><span data-stu-id="4ed0b-121">Music Store application will be built using **Model View Controller (MVC)**, an architectural pattern that separates an application into three main components:</span></span>

- <span data-ttu-id="4ed0b-122">**Modele**: obiekty modelu są częściami aplikacji, które implementują logikę domeny.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-122">**Models**: Model objects are the parts of the application that implement the domain logic.</span></span> <span data-ttu-id="4ed0b-123">Często obiekty modelu również pobierają i przechowują stan modelu w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-123">Often, model objects also retrieve and store model state in a database.</span></span>
- <span data-ttu-id="4ed0b-124">**Widoki:** Widoki to składniki, które wyświetlają interfejs użytkownika aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-124">**Views:** Views are the components that display the application's user interface (UI).</span></span> <span data-ttu-id="4ed0b-125">Zazwyczaj interfejs ten jest tworzony na podstawie danych modelu.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-125">Typically, this UI is created from the model data.</span></span> <span data-ttu-id="4ed0b-126">Przykładem może być widok do edycji albumów, który wyświetla pola tekstowe i listę rozwijaną na podstawie bieżącego stanu obiektu albumu.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-126">An example would be the edit view of Albums that displays text boxes and a drop-down list based on the current state of an Album object.</span></span>
- <span data-ttu-id="4ed0b-127">**Kontrolery:** Kontrolery to składniki obsługujące interakcję z użytkownikiem, manipulowanie modelem i ostatecznie Wybieranie widoku do renderowania interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-127">**Controllers:** Controllers are the components that handle user interaction, manipulate the model, and ultimately select a view to render the UI.</span></span> <span data-ttu-id="4ed0b-128">W aplikacji MVC widok służy wyłącznie do wyświetlania informacji. Za obsługę danych wprowadzanych przez użytkownika i interakcję z użytkownikiem odpowiada kontroler.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-128">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="4ed0b-129">Wzorzec MVC ułatwia tworzenie aplikacji, które oddzielają różne aspekty aplikacji (logiki wejściowej, logiki biznesowej i logiki interfejsu użytkownika), jednocześnie zapewniając swobodny sprzężenie między tymi elementami.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-129">The MVC pattern helps you to create applications that separate the different aspects of the application (input logic, business logic, and UI logic), while providing a loose coupling between these elements.</span></span> <span data-ttu-id="4ed0b-130">Ta separacja ułatwia zarządzanie złożonością podczas kompilowania aplikacji, ponieważ umożliwia skoncentrowanie się na jednym z aspektów implementacji w danym momencie.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-130">This separation helps you manage complexity when you build an application, as it allows you to focus on one aspect of the implementation at a time.</span></span> <span data-ttu-id="4ed0b-131">Ponadto wzorzec MVC ułatwia testowanie aplikacji, ułatwiając korzystanie z programowania opartego na testach (TDD) do tworzenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-131">In addition, the MVC pattern makes it easy to test applications, also encouraging the use of test-driven development (TDD) for creating applications.</span></span>

<span data-ttu-id="4ed0b-132">Struktura **ASP.NET MVC** stanowi alternatywę dla wzorca formularzy sieci Web ASP.NET do tworzenia ASP.NET aplikacji sieci Web opartych na MVC.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-132">The **ASP.NET MVC** framework provides an alternative to the ASP.NET Web Forms pattern for creating ASP.NET MVC-based Web applications.</span></span> <span data-ttu-id="4ed0b-133">**ASP.NET MVC** Framework jest lekkim i wysoce weryfikowalne strukturą prezentacji, która (podobnie jak w przypadku aplikacji opartych na formularzach internetowych) jest zintegrowana z istniejącymi funkcjami ASP.NET, takimi jak strony główne i uwierzytelnianie oparte na członkostwie, dzięki czemu można korzystać z zalet podstawowego programu .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-133">The **ASP.NET MVC** framework is a lightweight, highly testable presentation framework that (as with Web-forms-based applications) is integrated with existing ASP.NET features, such as master pages and membership-based authentication so you get all the power of the core .NET framework.</span></span> <span data-ttu-id="4ed0b-134">Jest to przydatne, jeśli znasz już ASP.NET formularze sieci Web, ponieważ wszystkie używane biblioteki są również dostępne w ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-134">This is useful if you are already familiar with ASP.NET Web Forms because all the libraries that you already use are available in ASP.NET MVC 4 as well.</span></span>

<span data-ttu-id="4ed0b-135">Ponadto luźne sprzęganie między trzema głównymi składnikami aplikacji MVC również wspiera programowanie równoległe.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-135">In addition, the loose coupling between the three main components of an MVC application also promotes parallel development.</span></span> <span data-ttu-id="4ed0b-136">Na przykład jeden deweloper może pracować w widoku, drugi deweloper może pracować na logice kontrolera, a trzeci deweloper może skupić się na logice biznesowej w modelu.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-136">For instance, one developer can work on the view, a second developer can work on the controller logic, and a third developer can focus on the business logic in the model.</span></span>

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="4ed0b-137">Cele</span><span class="sxs-lookup"><span data-stu-id="4ed0b-137">Objectives</span></span>

<span data-ttu-id="4ed0b-138">W tym ćwiczeniu dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="4ed0b-138">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="4ed0b-139">Tworzenie aplikacji ASP.NET MVC od podstaw w oparciu o samouczek aplikacji do sklepu muzycznego</span><span class="sxs-lookup"><span data-stu-id="4ed0b-139">Create an ASP.NET MVC application from scratch based on the Music Store Application tutorial</span></span>
- <span data-ttu-id="4ed0b-140">Dodaj kontrolery, aby obsługiwać adresy URL na stronie głównej witryny i przeglądać jej główne funkcje</span><span class="sxs-lookup"><span data-stu-id="4ed0b-140">Add Controllers to handle URLs to the Home page of the site and for browsing its main functionality</span></span>
- <span data-ttu-id="4ed0b-141">Dodaj widok, aby dostosować wyświetlaną zawartość wraz z jej stylem</span><span class="sxs-lookup"><span data-stu-id="4ed0b-141">Add a View to customize the content displayed along with its style</span></span>
- <span data-ttu-id="4ed0b-142">Dodawanie klas modelu w celu uwzględnienia danych i logiki domeny oraz zarządzania nimi</span><span class="sxs-lookup"><span data-stu-id="4ed0b-142">Add Model classes to contain and manage data and domain logic</span></span>
- <span data-ttu-id="4ed0b-143">Używanie wzorca widoku do przekazywania informacji z akcji kontrolera do szablonów widoków</span><span class="sxs-lookup"><span data-stu-id="4ed0b-143">Use View Model pattern to pass information from controller actions to the view templates</span></span>
- <span data-ttu-id="4ed0b-144">Poznaj nowy szablon ASP.NET MVC 4 dla aplikacji internetowych</span><span class="sxs-lookup"><span data-stu-id="4ed0b-144">Explore the ASP.NET MVC 4 new template for internet applications</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="4ed0b-145">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="4ed0b-145">Prerequisites</span></span>

<span data-ttu-id="4ed0b-146">Aby ukończyć to laboratorium, musisz mieć następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="4ed0b-146">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="4ed0b-147">[Visual Studio 2012 Express for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (Przeczytaj [dodatek A,](#AppendixA) Aby uzyskać instrukcje dotyczące sposobu jego instalacji)</span><span class="sxs-lookup"><span data-stu-id="4ed0b-147">[Visual Studio 2012 Express for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (read [Appendix A](#AppendixA) for instructions on how to install it)</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="4ed0b-148">Konfigurowanie</span><span class="sxs-lookup"><span data-stu-id="4ed0b-148">Setup</span></span>

<span data-ttu-id="4ed0b-149">**Instalowanie fragmentów kodu**</span><span class="sxs-lookup"><span data-stu-id="4ed0b-149">**Installing Code Snippets**</span></span>

<span data-ttu-id="4ed0b-150">Dla wygody większość kodu, który będzie zarządzany w tym laboratorium, jest dostępna jako fragmenty kodu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-150">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="4ed0b-151">Aby zainstalować plik fragmentów kodu, uruchom **.\Source\Setup\CodeSnippets.VSI** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-151">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="4ed0b-152">Jeśli nie znasz fragmentów kodu Visual Studio Code i chcesz dowiedzieć się, jak z nich korzystać, możesz odwołać się do dodatku z tego dokumentu &quot;[dodatku C: Using fragmenty kodu](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-152">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="4ed0b-153">Symulacyjn</span><span class="sxs-lookup"><span data-stu-id="4ed0b-153">Exercises</span></span>

<span data-ttu-id="4ed0b-154">To laboratorium praktyczne obejmuje następujące ćwiczenia:</span><span class="sxs-lookup"><span data-stu-id="4ed0b-154">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="4ed0b-155">Ćwiczenie 1: Tworzenie projektu aplikacji sieci Web MusicStore ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="4ed0b-155">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>](#Exercise1)
2. [<span data-ttu-id="4ed0b-156">Ćwiczenie 2: Tworzenie kontrolera</span><span class="sxs-lookup"><span data-stu-id="4ed0b-156">Exercise 2: Creating a Controller</span></span>](#Exercise2)
3. [<span data-ttu-id="4ed0b-157">Ćwiczenie 3: przekazywanie parametrów do kontrolera</span><span class="sxs-lookup"><span data-stu-id="4ed0b-157">Exercise 3: Passing parameters to a Controller</span></span>](#Exercise3)
4. [<span data-ttu-id="4ed0b-158">Ćwiczenie 4: Tworzenie widoku</span><span class="sxs-lookup"><span data-stu-id="4ed0b-158">Exercise 4: Creating a View</span></span>](#Exercise4)
5. [<span data-ttu-id="4ed0b-159">Ćwiczenie 5. Tworzenie modelu widoku</span><span class="sxs-lookup"><span data-stu-id="4ed0b-159">Exercise 5: Creating a View Model</span></span>](#Exercise5)
6. [<span data-ttu-id="4ed0b-160">Ćwiczenie 6: Używanie parametrów w widoku</span><span class="sxs-lookup"><span data-stu-id="4ed0b-160">Exercise 6: Using parameters in View</span></span>](#Exercise6)
7. [<span data-ttu-id="4ed0b-161">Ćwiczenie 7: kolano wokół ASP.NET MVC 4 nowy szablon</span><span class="sxs-lookup"><span data-stu-id="4ed0b-161">Exercise 7: A lap around ASP.NET MVC 4 New Template</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="4ed0b-162">Każdemu ćwiczeniu towarzyszy folder **końcowy** zawierający rozwiązanie, które należy uzyskać po zakończeniu ćwiczenia.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-162">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="4ed0b-163">Tego rozwiązania można użyć jako przewodnika, jeśli potrzebujesz dodatkowej pomocy w pracy przez ćwiczenia.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-163">You can use this solution as a guide if you need additional help working through the exercises.</span></span>

<span data-ttu-id="4ed0b-164">Szacowany czas wykonywania tego laboratorium: **60 minut**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-164">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a><span data-ttu-id="4ed0b-165">Ćwiczenie 1: Tworzenie projektu aplikacji sieci Web MusicStore ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="4ed0b-165">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>

<span data-ttu-id="4ed0b-166">W tym ćwiczeniu dowiesz się, jak utworzyć aplikację ASP.NET MVC w programie Visual Studio 2012 Express for Web oraz jej organizacji głównego folderu.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-166">In this exercise, you will learn how to create an ASP.NET MVC application in Visual Studio 2012 Express for Web as well as its main folder organization.</span></span> <span data-ttu-id="4ed0b-167">Ponadto dowiesz się, jak dodać nowy kontroler i wyświetlić prosty ciąg na stronie głównej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-167">Additionally, you will learn how to add a new Controller and make it display a simple string in the application's home page.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a><span data-ttu-id="4ed0b-168">Zadanie 1 — Tworzenie projektu aplikacji sieci Web ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="4ed0b-168">Task 1 - Creating the ASP.NET MVC Web Application Project</span></span>

1. <span data-ttu-id="4ed0b-169">W tym zadaniu utworzysz pusty projekt aplikacji ASP.NET MVC przy użyciu szablonu MVC Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-169">In this task, you will create an empty ASP.NET MVC application project using the MVC Visual Studio template.</span></span> <span data-ttu-id="4ed0b-170">Rozpocznij **vs Express for Web**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-170">Start **VS Express for Web**.</span></span>
2. <span data-ttu-id="4ed0b-171">W menu **plik** kliknij pozycję **Nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-171">On the **File** menu, click **New Project**.</span></span>
3. <span data-ttu-id="4ed0b-172">W oknie dialogowym **Nowy projekt** wybierz typ projektu **aplikacja sieci Web MVC 4 ASP.NET** , który znajduje się w obszarze **Wizualizacja C#,** Lista szablonów **sieci Web** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-172">In the **New Project** dialog box select the **ASP.NET MVC 4 Web Application** project type, located under **Visual C#,** **Web** template list.</span></span>
4. <span data-ttu-id="4ed0b-173">Zmień **nazwę** na *MvcMusicStore*.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-173">Change the **Name** to *MvcMusicStore*.</span></span>
5. <span data-ttu-id="4ed0b-174">Ustaw lokalizację rozwiązania w nowym folderze **BEGIN** w folderze źródłowym tego ćwiczenia, na przykład **[Twoje-hol-path] \Source\Ex01-CreatingMusicStoreProject\Begin**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-174">Set the location of the solution inside a new **Begin** folder in this Exercise's Source folder, for example **[YOUR-HOL-PATH]\Source\Ex01-CreatingMusicStoreProject\Begin**.</span></span> <span data-ttu-id="4ed0b-175">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-175">Click **OK**.</span></span>

    <span data-ttu-id="4ed0b-176">![Okno dialogowe Tworzenie nowego projektu](aspnet-mvc-4-fundamentals/_static/image2.png "Okno dialogowe Tworzenie nowego projektu")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-176">![Create New Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image2.png "Create New Project Dialog Box")</span></span>

    <span data-ttu-id="4ed0b-177">*Okno dialogowe Tworzenie nowego projektu*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-177">*Create New Project Dialog Box*</span></span>
6. <span data-ttu-id="4ed0b-178">W oknie dialogowym **Nowy projekt ASP.NET MVC 4** wybierz szablon **podstawowy** i upewnij się, że wybrany **aparat widoku** to **Razor**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-178">In the **New ASP.NET MVC 4 Project** dialog box select the **Basic** template and make sure that the **View engine** selected is **Razor**.</span></span> <span data-ttu-id="4ed0b-179">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-179">Click **OK**.</span></span>

    <span data-ttu-id="4ed0b-180">![Okno dialogowe Nowy projekt ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image3.png "Okno dialogowe Nowy projekt ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-180">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image3.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="4ed0b-181">*Okno dialogowe Nowy projekt ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-181">*New ASP.NET MVC 4 Project Dialog Box*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a><span data-ttu-id="4ed0b-182">Zadanie 2 — Eksplorowanie struktury rozwiązania</span><span class="sxs-lookup"><span data-stu-id="4ed0b-182">Task 2 - Exploring the Solution Structure</span></span>

<span data-ttu-id="4ed0b-183">Struktura ASP.NET MVC obejmuje szablon projektu programu Visual Studio, który ułatwia tworzenie aplikacji sieci Web obsługujących wzorzec MVC.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-183">The ASP.NET MVC framework includes a Visual Studio project template that helps you create Web applications supporting the MVC pattern.</span></span> <span data-ttu-id="4ed0b-184">Ten szablon służy do tworzenia nowej aplikacji sieci Web ASP.NET MVC z wymaganymi folderami, szablonami elementów i wpisami plików konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-184">This template creates a new ASP.NET MVC Web application with the required folders, item templates, and configuration-file entries.</span></span>

<span data-ttu-id="4ed0b-185">W tym zadaniu sprawdzisz strukturę rozwiązania, aby poznać elementy, które są objęte usługą, i ich relacje.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-185">In this task, you will examine the solution structure to understand the elements that are involved and their relationships.</span></span> <span data-ttu-id="4ed0b-186">Następujące foldery są dołączone do wszystkich aplikacji ASP.NET MVC, ponieważ struktura ASP.NET MVC domyślnie używa konwencji &quot;w porównaniu z podejściem&quot; konfiguracji i wprowadza pewne domyślne założenia na podstawie konwencji nazewnictwa folderów.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-186">The following folders are included in all the ASP.NET MVC application because the ASP.NET MVC framework by default uses a &quot;convention over configuration&quot; approach, and makes some default assumptions based on folder naming conventions.</span></span>

1. <span data-ttu-id="4ed0b-187">Po utworzeniu projektu Przejrzyj strukturę folderów utworzoną w Eksplorator rozwiązań po prawej stronie:</span><span class="sxs-lookup"><span data-stu-id="4ed0b-187">Once the project is created, review the folder structure that has been created in the Solution Explorer on the right side:</span></span>

    <span data-ttu-id="4ed0b-188">![Struktura folderu ASP.NET MVC w Eksplorator rozwiązań](aspnet-mvc-4-fundamentals/_static/image4.png "Struktura folderu ASP.NET MVC w Eksplorator rozwiązań")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-188">![ASP.NET MVC Folder structure in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image4.png "ASP.NET MVC Folder structure in Solution Explorer")</span></span>

    <span data-ttu-id="4ed0b-189">*Struktura folderu ASP.NET MVC w Eksplorator rozwiązań*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-189">*ASP.NET MVC Folder structure in Solution Explorer*</span></span>

   1. <span data-ttu-id="4ed0b-190">**Kontrolery**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-190">**Controllers**.</span></span> <span data-ttu-id="4ed0b-191">Ten folder będzie zawierać klasy kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-191">This folder will contain the controller classes.</span></span> <span data-ttu-id="4ed0b-192">W aplikacji opartej na platformie MVC kontrolery są odpowiedzialne za obsługę interakcji z użytkownikami końcowymi, manipulowanie modelem i ostatecznie Wybieranie widoku w celu renderowania interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-192">In an MVC based application, controllers are responsible for handling end user interaction, manipulating the model, and ultimately choosing a view to render the UI.</span></span>

       > [!NOTE]
       > <span data-ttu-id="4ed0b-193">Struktura MVC wymaga, aby nazwy wszystkich kontrolerów były kończyć się &quot;kontrolerem&quot;— na przykład HomeController, LoginController lub ProductController.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-193">The MVC framework requires the names of all controllers to end with &quot;Controller&quot;-for example, HomeController, LoginController, or ProductController.</span></span>
   2. <span data-ttu-id="4ed0b-194">**Modele**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-194">**Models**.</span></span> <span data-ttu-id="4ed0b-195">Ten folder jest udostępniany dla klas, które reprezentują model aplikacji dla aplikacji sieci Web MVC.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-195">This folder is provided for classes that represent the application model for the MVC Web application.</span></span> <span data-ttu-id="4ed0b-196">Obejmuje to zazwyczaj kod definiujący obiekty i logikę do współpracy z magazynem danych.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-196">This usually includes code that defines objects and the logic for interacting with the data store.</span></span> <span data-ttu-id="4ed0b-197">Zazwyczaj rzeczywiste obiekty modelu będą znajdować się w osobnych bibliotekach klas.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-197">Typically, the actual model objects will be in separate class libraries.</span></span> <span data-ttu-id="4ed0b-198">Jednak podczas tworzenia nowej aplikacji można uwzględnić klasy, a następnie przenieść je do oddzielnych bibliotek klas w późniejszym czasie w cyklu projektowania.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-198">However, when you create a new application, you might include classes and then move them into separate class libraries at a later point in the development cycle.</span></span>
   3. <span data-ttu-id="4ed0b-199">**Widoki**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-199">**Views**.</span></span> <span data-ttu-id="4ed0b-200">Ten folder jest zalecaną lokalizacją widoków, składnikami odpowiedzialnymi za wyświetlanie interfejsu użytkownika aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-200">This folder is the recommended location for views, the components responsible for displaying the application's user interface.</span></span> <span data-ttu-id="4ed0b-201">Widoki używają plików. aspx,. ascx,. cshtml i. Master, oprócz innych plików, które są powiązane z widokami renderowania.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-201">Views use .aspx, .ascx, .cshtml and .master files, in addition to any other files that are related to rendering views.</span></span> <span data-ttu-id="4ed0b-202">Folder widoki zawiera folder dla każdego kontrolera; folder nosi nazwę z prefiksem nazwy kontrolera.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-202">Views folder contains a folder for each controller; the folder is named with the controller-name prefix.</span></span> <span data-ttu-id="4ed0b-203">Na przykład jeśli masz kontroler o nazwie **HomeController**, folder widoki będzie zawierać folder o nazwie Home.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-203">For example, if you have a controller named **HomeController**, the Views folder will contain a folder named Home.</span></span> <span data-ttu-id="4ed0b-204">Domyślnie, gdy struktura ASP.NET MVC ładuje widok, szuka pliku. aspx z żądaną nazwą widoku w folderze Views\controllerName (**widoki [ControllerName] [Action]. aspx**) lub (**widoki [ControllerName] [Action]. cshtml**) dla widoków Razor.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-204">By default, when the ASP.NET MVC framework loads a view, it looks for an .aspx file with the requested view name in the Views\controllerName folder (**Views[ControllerName][Action].aspx**) or (**Views[ControllerName][Action].cshtml**) for Razor Views.</span></span>

      > [!NOTE]
      > <span data-ttu-id="4ed0b-205">Oprócz folderów wymienionych wcześniej aplikacja sieci Web MVC używa pliku **Global. asax** do ustawiania ustawień globalnych routingu adresów URL i używa pliku **Web. config** w celu skonfigurowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-205">In addition to the folders listed previously, an MVC Web application uses the **Global.asax** file to set global URL routing defaults, and it uses the **Web.config** file to configure the application.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a><span data-ttu-id="4ed0b-206">Zadanie 3 — Dodawanie elementu HomeController</span><span class="sxs-lookup"><span data-stu-id="4ed0b-206">Task 3 - Adding a HomeController</span></span>

<span data-ttu-id="4ed0b-207">W aplikacjach ASP.NET, które nie korzystają z platformy MVC, interakcja użytkownika jest zorganizowana wokół stron i wokół zdarzeń z tych stron.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-207">In ASP.NET applications that do not use the MVC framework, user interaction is organized around pages, and around raising and handling events from those pages.</span></span> <span data-ttu-id="4ed0b-208">Z kolei Interakcje użytkownika z aplikacjami ASP.NET MVC są zorganizowane wokół kontrolerów i ich metod działania.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-208">In contrast, user interaction with ASP.NET MVC applications is organized around controllers and their action methods.</span></span>

<span data-ttu-id="4ed0b-209">Z drugiej strony ASP.NET MVC Framework mapuje adresy URL na klasy, które są określane jako kontrolery.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-209">On the other hand, ASP.NET MVC framework maps URLs to classes that are referred to as controllers.</span></span> <span data-ttu-id="4ed0b-210">Kontrolery przetwarzają żądania przychodzące, obsługują dane wejściowe użytkownika i interakcje, wykonują odpowiednią logikę aplikacji i określają odpowiedź do wysłania do klienta (wyświetla kod HTML, pobiera plik, przekierowuje do innego adresu URL itp.).</span><span class="sxs-lookup"><span data-stu-id="4ed0b-210">Controllers process incoming requests, handle user input and interactions, execute appropriate application logic and determine the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span> <span data-ttu-id="4ed0b-211">W przypadku wyświetlania kodu HTML Klasa kontrolera zazwyczaj wywołuje oddzielny składnik widoku w celu wygenerowania znacznika HTML dla żądania.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-211">In the case of displaying HTML, a controller class typically calls a separate view component to generate the HTML markup for the request.</span></span> <span data-ttu-id="4ed0b-212">W aplikacji MVC widok służy wyłącznie do wyświetlania informacji. Za obsługę danych wprowadzanych przez użytkownika i interakcję z użytkownikiem odpowiada kontroler.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-212">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="4ed0b-213">W tym zadaniu zostanie dodana klasa kontrolera, która będzie obsługiwać adresy URL na stronie głównej witryny sklepu muzyczne.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-213">In this task, you will add a Controller class that will handle URLs to the Home page of the Music Store site.</span></span>

1. <span data-ttu-id="4ed0b-214">Kliknij prawym przyciskiem myszy folder **controllers** w obszarze Eksplorator rozwiązań wybierz pozycję **Dodaj** , a następnie polecenie **Controller** :</span><span class="sxs-lookup"><span data-stu-id="4ed0b-214">Right-click **Controllers** folder within the Solution Explorer, select **Add** and then **Controller** command:</span></span>

    <span data-ttu-id="4ed0b-215">![Dodaj polecenie kontrolera](aspnet-mvc-4-fundamentals/_static/image5.png "Dodaj polecenie kontrolera")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-215">![Add a Controller Command](aspnet-mvc-4-fundamentals/_static/image5.png "Add a Controller Command")</span></span>

    <span data-ttu-id="4ed0b-216">*Dodaj kontroler — polecenie*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-216">*Add Controller Command*</span></span>
2. <span data-ttu-id="4ed0b-217">Zostanie wyświetlone okno dialogowe **Dodaj kontroler** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-217">The **Add Controller** dialog appears.</span></span> <span data-ttu-id="4ed0b-218">Nazwij *HomeController* kontrolera i naciśnij przycisk **Add (Dodaj**).</span><span class="sxs-lookup"><span data-stu-id="4ed0b-218">Name the controller *HomeController* and press **Add**.</span></span>

    <span data-ttu-id="4ed0b-219">![Okno dialogowe Dodawanie kontrolera](aspnet-mvc-4-fundamentals/_static/image6.png "Okno dialogowe Dodawanie kontrolera")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-219">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image6.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="4ed0b-220">*Okno dialogowe Dodawanie kontrolera*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-220">*Add Controller Dialog*</span></span>
3. <span data-ttu-id="4ed0b-221">Plik **HomeController.cs** jest tworzony w folderze **controllers** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-221">The file **HomeController.cs** is created in the **Controllers** folder.</span></span> <span data-ttu-id="4ed0b-222">Aby **HomeController** zwracał ciąg w akcji indeksu, Zastąp metodę **index** następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="4ed0b-222">In order to have the **HomeController** return a string on its Index action, replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="4ed0b-223">(Fragment kodu — *ASP.NET MVC 4 — podstawowe-Ex1 HomeController index*)</span><span class="sxs-lookup"><span data-stu-id="4ed0b-223">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex1 HomeController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="4ed0b-224">Zadanie 4 — Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="4ed0b-224">Task 4 - Running the Application</span></span>

<span data-ttu-id="4ed0b-225">To zadanie spowoduje wypróbowanie aplikacji w przeglądarce sieci Web.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-225">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="4ed0b-226">Naciśnij klawisz **F5** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-226">Press **F5** to run the Application.</span></span> <span data-ttu-id="4ed0b-227">Projekt jest kompilowany i uruchamiany jest lokalny serwer sieci Web usług IIS.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-227">The project is compiled and the local IIS Web Server starts.</span></span> <span data-ttu-id="4ed0b-228">Lokalny serwer sieci Web usług IIS automatycznie otworzy przeglądarkę internetową wskazującą adres URL serwera sieci Web.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-228">The local IIS Web Server will automatically open a web browser pointing to the URL of the Web server.</span></span>

    <span data-ttu-id="4ed0b-229">![Aplikacja działająca w przeglądarce sieci Web](aspnet-mvc-4-fundamentals/_static/image7.png "Aplikacja działająca w przeglądarce sieci Web")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-229">![Application running in a web browser](aspnet-mvc-4-fundamentals/_static/image7.png "Application running in a web browser")</span></span>

    <span data-ttu-id="4ed0b-230">*Aplikacja działająca w przeglądarce sieci Web*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-230">*Application running in a web browser*</span></span>

    > [!NOTE]
    > <span data-ttu-id="4ed0b-231">Lokalny serwer sieci Web usług IIS uruchomi witrynę internetową przy użyciu losowego numeru portu.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-231">The local IIS Web Server will run the website on a random free port number.</span></span> <span data-ttu-id="4ed0b-232">Na powyższej ilustracji lokacja jest uruchomiona w `http://localhost:50103/`, więc używa portu 50103.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-232">In the figure above, the site is running at `http://localhost:50103/`, so it's using port 50103.</span></span> <span data-ttu-id="4ed0b-233">Numer portu może się różnić.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-233">Your port number may vary.</span></span>
2. <span data-ttu-id="4ed0b-234">Zamknij okno przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-234">Close the browser.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a><span data-ttu-id="4ed0b-235">Ćwiczenie 2: Tworzenie kontrolera</span><span class="sxs-lookup"><span data-stu-id="4ed0b-235">Exercise 2: Creating a Controller</span></span>

<span data-ttu-id="4ed0b-236">W tym ćwiczeniu dowiesz się, jak zaktualizować kontroler w celu zaimplementowania prostych funkcji aplikacji do sklepu muzycznego.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-236">In this exercise, you will learn how to update the controller to implement simple functionality of the Music Store application.</span></span> <span data-ttu-id="4ed0b-237">Ten kontroler określi metody akcji do obsługi każdego z następujących konkretnych żądań:</span><span class="sxs-lookup"><span data-stu-id="4ed0b-237">That controller will define action methods to handle each of the following specific requests:</span></span>

- <span data-ttu-id="4ed0b-238">Strona z listą gatunku muzyka w sklepie Music</span><span class="sxs-lookup"><span data-stu-id="4ed0b-238">A listing page of the music genres in the Music Store</span></span>
- <span data-ttu-id="4ed0b-239">Strona przeglądania zawierająca listę wszystkich albumów muzycznych dla określonego gatunku</span><span class="sxs-lookup"><span data-stu-id="4ed0b-239">A browse page that lists all of the music albums for a particular genre</span></span>
- <span data-ttu-id="4ed0b-240">Strona szczegółów, która zawiera informacje o konkretnym albumie muzycznym</span><span class="sxs-lookup"><span data-stu-id="4ed0b-240">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="4ed0b-241">Dla zakresu tego ćwiczenia te akcje będą po prostu zwracać ciąg przez teraz.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-241">For the scope of this exercise, those actions will simply return a string by now.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a><span data-ttu-id="4ed0b-242">Zadanie 1 — Dodawanie nowej klasy StoreController</span><span class="sxs-lookup"><span data-stu-id="4ed0b-242">Task 1 - Adding a New StoreController Class</span></span>

<span data-ttu-id="4ed0b-243">W tym zadaniu zostanie dodany nowy kontroler.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-243">In this task, you will add a new Controller.</span></span>

1. <span data-ttu-id="4ed0b-244">Jeśli nie jest jeszcze otwarty, uruchom **VS Express for Web 2012**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-244">If not already open, start **VS Express for Web 2012**.</span></span>
2. <span data-ttu-id="4ed0b-245">W menu **plik** wybierz **Otwórz projekt**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-245">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="4ed0b-246">W oknie dialogowym Otwórz projekt przejdź do **Source\Ex02-CreatingAController\Begin**, wybierz pozycję **Rozpocznij. sln** , a następnie kliknij pozycję **Otwórz**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-246">In the Open Project dialog, browse to **Source\Ex02-CreatingAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="4ed0b-247">Alternatywnie można nadal korzystać z rozwiązania uzyskanego po zakończeniu poprzedniego ćwiczenia.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-247">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="4ed0b-248">Jeśli otwarto **udostępnione rozwiązanie** , należy pobrać brakujące pakiety NuGet przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-248">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="4ed0b-249">W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-249">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="4ed0b-250">W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-250">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="4ed0b-251">Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-251">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="4ed0b-252">Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-252">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="4ed0b-253">Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-253">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="4ed0b-254">To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-254">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="4ed0b-255">Dodaj nowy kontroler.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-255">Add the new controller.</span></span> <span data-ttu-id="4ed0b-256">Aby to zrobić, kliknij prawym przyciskiem myszy folder **controllers** w obszarze Eksplorator rozwiązań wybierz pozycję **Dodaj** , a następnie polecenie **kontroler** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-256">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="4ed0b-257">Zmień **nazwę kontrolera** na *StoreController*, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-257">Change the **Controller Name** to *StoreController*, and click **Add**.</span></span>

    <span data-ttu-id="4ed0b-258">![Okno dialogowe Dodawanie kontrolera](aspnet-mvc-4-fundamentals/_static/image8.png "Okno dialogowe Dodawanie kontrolera")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-258">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image8.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="4ed0b-259">*Okno dialogowe Dodawanie kontrolera*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-259">*Add Controller Dialog*</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a><span data-ttu-id="4ed0b-260">Zadanie 2 — Modyfikowanie akcji StoreController</span><span class="sxs-lookup"><span data-stu-id="4ed0b-260">Task 2 - Modifying the StoreController's Actions</span></span>

<span data-ttu-id="4ed0b-261">W tym zadaniu zmodyfikujesz metody kontrolera, które są nazywane **akcjami**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-261">In this task, you will modify the Controller methods that are called **actions**.</span></span> <span data-ttu-id="4ed0b-262">Akcje są odpowiedzialne za obsługę żądań adresów URL i Określanie zawartości, która powinna zostać wysłana z powrotem do przeglądarki lub użytkownika, który wywołał adres URL.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-262">Actions are responsible for handling URL requests and determining the content that should be sent back to the browser or user that invoked the URL.</span></span>

1. <span data-ttu-id="4ed0b-263">Klasa **StoreController** ma już metodę **index** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-263">The **StoreController** class already has an **Index** method.</span></span> <span data-ttu-id="4ed0b-264">Zostanie ona użyta w dalszej części tego laboratorium do zaimplementowania strony, która wyświetla listę wszystkich gatunków sklepu muzycznego.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-264">You will use it later in this Lab to implement the page that lists all genres of the music store.</span></span> <span data-ttu-id="4ed0b-265">Na razie po prostu Zastąp metodę **index** następującym kodem, który zwraca ciąg &quot;Hello z elementu Store. index ()&quot;:</span><span class="sxs-lookup"><span data-stu-id="4ed0b-265">For now, just replace the **Index** method with the following code that returns a string &quot;Hello from Store.Index()&quot;:</span></span>

    <span data-ttu-id="4ed0b-266">(Fragment kodu — *ASP.NET MVC 4 — podstawowe-Ex2 StoreController index*)</span><span class="sxs-lookup"><span data-stu-id="4ed0b-266">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. <span data-ttu-id="4ed0b-267">Dodawanie metod **przeglądania** i **szczegółów** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-267">Add **Browse** and **Details** methods.</span></span> <span data-ttu-id="4ed0b-268">Aby to zrobić, Dodaj następujący kod do **StoreController**:</span><span class="sxs-lookup"><span data-stu-id="4ed0b-268">To do this, add the following code to the **StoreController**:</span></span>

    <span data-ttu-id="4ed0b-269">(Fragment kodu — *ASP.NET MVC 4 — podstawowe-Ex2 StoreController BrowseAndDetails*)</span><span class="sxs-lookup"><span data-stu-id="4ed0b-269">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController BrowseAndDetails*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="4ed0b-270">Zadanie 3 — Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="4ed0b-270">Task 3 - Running the Application</span></span>

<span data-ttu-id="4ed0b-271">To zadanie spowoduje wypróbowanie aplikacji w przeglądarce sieci Web.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-271">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="4ed0b-272">Naciśnij klawisz **F5** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-272">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="4ed0b-273">Projekt zostanie uruchomiony na stronie **głównej** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-273">The project starts in the **Home** page.</span></span> <span data-ttu-id="4ed0b-274">Zmień adres URL, aby zweryfikować implementację każdej akcji.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-274">Change the URL to verify each action's implementation.</span></span>

    1. <span data-ttu-id="4ed0b-275">**/Store**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-275">**/Store**.</span></span> <span data-ttu-id="4ed0b-276">Zobaczysz, **&quot;Hello z&quot;Store. index ()** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-276">You will see **&quot;Hello from Store.Index()&quot;**.</span></span>
    2. <span data-ttu-id="4ed0b-277">**/Store/Browse**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-277">**/Store/Browse**.</span></span> <span data-ttu-id="4ed0b-278">Zostanie wyświetlona **&quot;Hello ze sklepu. Browse ()&quot;** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-278">You will see **&quot;Hello from Store.Browse()&quot;**.</span></span>
    3. <span data-ttu-id="4ed0b-279">**/Store/Details**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-279">**/Store/Details**.</span></span> <span data-ttu-id="4ed0b-280">Zobaczysz, **&quot;Hello ze sklepu. Details ()&quot;** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-280">You will see **&quot;Hello from Store.Details()&quot;**.</span></span>

        <span data-ttu-id="4ed0b-281">![Przeglądanie StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "Przeglądanie StoreBrowse")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-281">![Browsing StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "Browsing StoreBrowse")</span></span>

        <span data-ttu-id="4ed0b-282">*Przeglądanie/Store/Browse*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-282">*Browsing /Store/Browse*</span></span>
3. <span data-ttu-id="4ed0b-283">Zamknij okno przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-283">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a><span data-ttu-id="4ed0b-284">Ćwiczenie 3: przekazywanie parametrów do kontrolera</span><span class="sxs-lookup"><span data-stu-id="4ed0b-284">Exercise 3: Passing parameters to a Controller</span></span>

<span data-ttu-id="4ed0b-285">Do tej pory zostały zwrócone ciągi stałe z kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-285">Until now, you have been returning constant strings from the Controllers.</span></span> <span data-ttu-id="4ed0b-286">W tym ćwiczeniu dowiesz się, jak przekazać parametry do kontrolera przy użyciu adresu URL i ciągu QueryString, a następnie wykonać akcje metody reagują na tekst do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-286">In this exercise you will learn how to pass parameters to a Controller using the URL and querystring, and then making the method actions respond with text to the browser.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a><span data-ttu-id="4ed0b-287">Zadanie 1 — Dodawanie parametru gatunku do StoreController</span><span class="sxs-lookup"><span data-stu-id="4ed0b-287">Task 1 - Adding Genre Parameter to StoreController</span></span>

<span data-ttu-id="4ed0b-288">W tym zadaniu zostanie użyta **Kolekcja QueryString** w celu wysłania parametrów do metody **Browse** Action w **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-288">In this task, you will use the **querystring** to send parameters to the **Browse** action method in the **StoreController**.</span></span>

1. <span data-ttu-id="4ed0b-289">Jeśli nie jest jeszcze otwarty, uruchom **vs Express for Web**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-289">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="4ed0b-290">W menu **plik** wybierz **Otwórz projekt**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-290">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="4ed0b-291">W oknie dialogowym Otwórz projekt przejdź do **Source\Ex03-PassingParametersToAController\Begin**, wybierz pozycję **Rozpocznij. sln** , a następnie kliknij pozycję **Otwórz**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-291">In the Open Project dialog, browse to **Source\Ex03-PassingParametersToAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="4ed0b-292">Alternatywnie można nadal korzystać z rozwiązania uzyskanego po zakończeniu poprzedniego ćwiczenia.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-292">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="4ed0b-293">Jeśli otwarto **udostępnione rozwiązanie** , należy pobrać brakujące pakiety NuGet przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-293">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="4ed0b-294">W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-294">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="4ed0b-295">W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-295">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="4ed0b-296">Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-296">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="4ed0b-297">Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-297">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="4ed0b-298">Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-298">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="4ed0b-299">To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-299">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="4ed0b-300">Otwórz klasę **StoreController** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-300">Open **StoreController** class.</span></span> <span data-ttu-id="4ed0b-301">W tym celu w **Eksplorator rozwiązań**rozwiń folder **Kontrolery** i kliknij dwukrotnie pozycję **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-301">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
4. <span data-ttu-id="4ed0b-302">Zmień metodę **przeglądania** , dodając parametr ciągu do żądania dla określonego gatunku.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-302">Change the **Browse** method, adding a string parameter to request for a specific genre.</span></span> <span data-ttu-id="4ed0b-303">ASP.NET MVC automatycznie przekaże wszystkie **Parametry w postaci** ciągu lub formularza post do tej metody akcji po wywołaniu.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-303">ASP.NET MVC will automatically pass any querystring or form post parameters named **genre** to this action method when invoked.</span></span> <span data-ttu-id="4ed0b-304">Aby to zrobić, Zastąp metodę **Browse** następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="4ed0b-304">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="4ed0b-305">(Fragment kodu — *ASP.NET MVC 4 — podstawowe-Ex3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="4ed0b-305">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="4ed0b-306">Używasz metody narzędziowej **HttpUtility. HtmlEncode** , aby uniemożliwiać użytkownikom wprowadzanie kodu JavaScript do widoku za pomocą linku, takiego jak **/Store/Browse? Gatunek =&lt;&gt;Window. Location = "[http://hackersite.com](http://hackersite.com)"&lt;/Script&gt;** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-306">You are using the **HttpUtility.HtmlEncode** utility method to prevents users from injecting Javascript into the View with a link like **/Store/Browse?Genre=&lt;script&gt;window.location='[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;**.</span></span>
> 
> <span data-ttu-id="4ed0b-307">Aby uzyskać więcej informacji, zobacz [ten artykuł w witrynie MSDN](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span><span class="sxs-lookup"><span data-stu-id="4ed0b-307">For further explanation, please visit [this msdn article](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="4ed0b-308">Zadanie 2 — Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="4ed0b-308">Task 2 - Running the Application</span></span>

<span data-ttu-id="4ed0b-309">To zadanie spowoduje wypróbowanie aplikacji w przeglądarce sieci Web i użycie parametru **gatunek** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-309">In this task, you will try out the Application in a web browser and use the **genre** parameter.</span></span>

1. <span data-ttu-id="4ed0b-310">Naciśnij klawisz **F5** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-310">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="4ed0b-311">Projekt zostanie uruchomiony na stronie **głównej** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-311">The project starts in the **Home** page.</span></span> <span data-ttu-id="4ed0b-312">Czy zmienić adres URL na */Store/Browse? Gatunek = Disco* , aby sprawdzić, czy akcja odbiera parametr gatunku.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-312">Change the URL to */Store/Browse?Genre=Disco* to verify that the action receives the genre parameter.</span></span>

    <span data-ttu-id="4ed0b-313">![Przeglądanie StoreBrowseGenre = Disco](aspnet-mvc-4-fundamentals/_static/image10.png "Przeglądanie StoreBrowseGenre = Disco")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-313">![Browsing StoreBrowseGenre=Disco](aspnet-mvc-4-fundamentals/_static/image10.png "Browsing StoreBrowseGenre=Disco")</span></span>

    <span data-ttu-id="4ed0b-314">*Przeglądasz/Store/Browse? Gatunek = Disco*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-314">*Browsing /Store/Browse?Genre=Disco*</span></span>
3. <span data-ttu-id="4ed0b-315">Zamknij okno przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-315">Close the browser.</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a><span data-ttu-id="4ed0b-316">Zadanie 3 — Dodawanie parametru identyfikatora osadzonego w adresie URL</span><span class="sxs-lookup"><span data-stu-id="4ed0b-316">Task 3 - Adding an Id Parameter Embedded in the URL</span></span>

<span data-ttu-id="4ed0b-317">W tym zadaniu zostanie użyty **adres URL** do przekazania parametru **ID** do metody akcji **Details** elementu **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-317">In this task, you will use the **URL** to pass an **Id** parameter to the **Details** action method of the **StoreController**.</span></span> <span data-ttu-id="4ed0b-318">Domyślna konwencja routingu ASP.NET MVC to traktowanie segmentu adresu URL po nazwie metody akcji jako parametru o nazwie **ID**. Jeśli metoda akcji ma parametr o nazwie ID, ASP.NET MVC automatycznie przekaże segment adresu URL jako parametr.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-318">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named **Id**. If your action method has parameter named Id, then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span> <span data-ttu-id="4ed0b-319">W polu Magazyn adresów URL **/szczegóły/5** **Identyfikator** będzie interpretowany jako **5**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-319">In the URL **Store/Details/5**, **Id** will be interpreted as **5**.</span></span>

1. <span data-ttu-id="4ed0b-320">Zmień metodę **Details** elementu **StoreController**, dodając parametr **int** o nazwie **ID**. Aby to zrobić, Zastąp metodę **Details** następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="4ed0b-320">Change the **Details** method of the **StoreController**, adding an **int** parameter called **id**. To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="4ed0b-321">(Fragment kodu — *ASP.NET MVC 4 — podstawowe-Ex3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="4ed0b-321">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="4ed0b-322">Zadanie 4 — Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="4ed0b-322">Task 4 - Running the Application</span></span>

<span data-ttu-id="4ed0b-323">To zadanie spowoduje wypróbowanie aplikacji w przeglądarce sieci Web i użycie parametru **ID** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-323">In this task, you will try out the Application in a web browser and use the **Id** parameter.</span></span>

1. <span data-ttu-id="4ed0b-324">Naciśnij klawisz **F5** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-324">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="4ed0b-325">Projekt zostanie uruchomiony na stronie **głównej** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-325">The project starts in the **Home** page.</span></span> <span data-ttu-id="4ed0b-326">Zmień adres URL na */Store/Details/5* , aby sprawdzić, czy akcja odbiera parametr ID.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-326">Change the URL to */Store/Details/5* to verify that the action receives the id parameter.</span></span>

    <span data-ttu-id="4ed0b-327">![Przeglądanie StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "Przeglądanie StoreDetails5")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-327">![Browsing StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "Browsing StoreDetails5")</span></span>

    <span data-ttu-id="4ed0b-328">*Przeglądanie/Store/Details/5*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-328">*Browsing /Store/Details/5*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a><span data-ttu-id="4ed0b-329">Ćwiczenie 4: Tworzenie widoku</span><span class="sxs-lookup"><span data-stu-id="4ed0b-329">Exercise 4: Creating a View</span></span>

<span data-ttu-id="4ed0b-330">Do tej pory zostały zwrócone ciągi z akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-330">So far you have been returning strings from controller actions.</span></span> <span data-ttu-id="4ed0b-331">Chociaż jest to przydatny sposób na zrozumienie sposobu działania kontrolerów, nie jest to sposób, w jaki są kompilowane prawdziwe aplikacje sieci Web.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-331">Although that is a useful way of understanding how controllers work, it is not how your real Web applications are built.</span></span> <span data-ttu-id="4ed0b-332">Widoki to składniki, które zapewniają lepsze podejście do generowania kodu HTML z powrotem do przeglądarki przy użyciu plików szablonów.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-332">Views are components that provide a better approach for generating HTML back to the browser with the use of template files.</span></span>

<span data-ttu-id="4ed0b-333">W tym ćwiczeniu dowiesz się, jak dodać stronę wzorcową układu, aby skonfigurować szablon dla typowej zawartości HTML, arkusz stylów, aby wzbogacić wygląd i działanie witryny, a wreszcie szablon widoku, aby umożliwić HomeController zwracanie kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-333">In this exercise you will learn how to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel of the site and finally a View template to enable HomeController to return HTML.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-_layoutcshtml"></a><span data-ttu-id="4ed0b-334">Zadanie 1 — Modyfikowanie pliku \_Layout. cshtml</span><span class="sxs-lookup"><span data-stu-id="4ed0b-334">Task 1 - Modifying the file \_layout.cshtml</span></span>

<span data-ttu-id="4ed0b-335">Plik **~/Views/Shared/\_Layout. cshtml** umożliwia skonfigurowanie szablonu do użycia w całej witrynie sieci Web.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-335">The file **~/Views/Shared/\_layout.cshtml** allows you to setup a template for common HTML to use across the entire website.</span></span> <span data-ttu-id="4ed0b-336">W tym zadaniu zostanie dodana Strona wzorcowa układu ze wspólnym nagłówkiem z linkami do strony głównej i obszaru magazynu.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-336">In this task you will add a layout master page with a common header with links to the Home page and Store area.</span></span>

1. <span data-ttu-id="4ed0b-337">Jeśli nie jest jeszcze otwarty, uruchom **vs Express for Web**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-337">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="4ed0b-338">W menu **plik** wybierz **Otwórz projekt**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-338">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="4ed0b-339">W oknie dialogowym Otwórz projekt przejdź do **Source\Ex04-CreatingAView\Begin**, wybierz pozycję **Rozpocznij. sln** , a następnie kliknij pozycję **Otwórz**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-339">In the Open Project dialog, browse to **Source\Ex04-CreatingAView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="4ed0b-340">Alternatywnie można nadal korzystać z rozwiązania uzyskanego po zakończeniu poprzedniego ćwiczenia.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-340">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="4ed0b-341">Jeśli otwarto **udostępnione rozwiązanie** , należy pobrać brakujące pakiety NuGet przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-341">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="4ed0b-342">W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-342">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="4ed0b-343">W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-343">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="4ed0b-344">Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-344">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="4ed0b-345">Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-345">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="4ed0b-346">Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-346">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="4ed0b-347">To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-347">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="4ed0b-348">Plik <strong>\_Layout. cshtml</strong> zawiera układ kontenera HTML dla wszystkich stron w witrynie.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-348">The file <strong>\_layout.cshtml</strong> contains the HTML container layout for all pages on the site.</span></span> <span data-ttu-id="4ed0b-349">Zawiera element <strong>&lt;html&gt;</strong> dla odpowiedzi HTML, a także <strong>&lt;&gt;</strong> i <strong>&lt;treść</strong>&gt;.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-349">It includes the <strong>&lt;html&gt;</strong> element for the HTML response, as well as the <strong>&lt;head&gt;</strong> and <strong>&lt;body&gt;</strong> elements.</span></span> <span data-ttu-id="4ed0b-350"><strong>@RenderBody()</strong> w treści HTML Zidentyfikuj regiony, w których szablony będą mogły być wypełniane zawartością dynamiczną.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-350"><strong>@RenderBody()</strong> within the HTML body identify regions that view templates will be able to fill in with dynamic content.</span></span>
   <span data-ttu-id="4ed0b-351">(C#)</span><span class="sxs-lookup"><span data-stu-id="4ed0b-351">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. <span data-ttu-id="4ed0b-352">Dodaj wspólny nagłówek z linkami do strony głównej i obszaru przechowywania na wszystkich stronach w witrynie.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-352">Add a common header with links to the Home page and Store area on all pages in the site.</span></span> <span data-ttu-id="4ed0b-353">Aby to zrobić, Dodaj następujący kod poniżej &lt;treści&gt; instrukcji.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-353">In order to do that, add the following code below &lt;body&gt; statement.</span></span>
   <span data-ttu-id="4ed0b-354">(C#)</span><span class="sxs-lookup"><span data-stu-id="4ed0b-354">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. <span data-ttu-id="4ed0b-355">Dołącz element DIV, aby renderować sekcję treści każdej strony.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-355">Include a div to render the body section of each page.</span></span> <span data-ttu-id="4ed0b-356">Zamień <strong>@RenderBody()</strong> na następujący wyróżniony kod: (C#)</span><span class="sxs-lookup"><span data-stu-id="4ed0b-356">Replace <strong>@RenderBody()</strong> with the following highlighted code: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="4ed0b-357">Czy wiesz?</span><span class="sxs-lookup"><span data-stu-id="4ed0b-357">Did you know?</span></span> <span data-ttu-id="4ed0b-358">Program Visual Studio 2012 zawiera fragmenty, które ułatwiają dodawanie często używanego kodu w języku HTML, pliki kodu i inne elementy.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-358">Visual Studio 2012 has snippets that make it easy to add commonly used code in HTML, code files and more!</span></span> <span data-ttu-id="4ed0b-359">Wypróbuj go, wpisując **&lt;div&gt;** i naciskając klawisz **Tab** dwa razy, aby wstawić kompletny tag **DIV** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-359">Try it out by typing **&lt;div&gt;** and pressing **TAB** twice to insert a complete **div** tag.</span></span>

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a><span data-ttu-id="4ed0b-360">Zadanie 2 — Dodawanie arkusza stylów CSS</span><span class="sxs-lookup"><span data-stu-id="4ed0b-360">Task 2 - Adding CSS Stylesheet</span></span>

<span data-ttu-id="4ed0b-361">Szablon pustego projektu zawiera bardzo uproszczony plik CSS, który zawiera tylko style używane do wyświetlania podstawowych formularzy i komunikatów weryfikacyjnych.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-361">The empty project template includes a very streamlined CSS file which just includes styles used to display basic forms and validation messages.</span></span> <span data-ttu-id="4ed0b-362">Będziesz używać dodatkowych arkuszy CSS i obrazów (potencjalnie oferowanych przez projektanta) w celu zwiększenia wyglądu i działania witryny.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-362">You will use additional CSS and images (potentially provided by a designer) in order to enhance the look and feel of the site.</span></span>

<span data-ttu-id="4ed0b-363">W tym zadaniu dodasz arkusz stylów CSS do definiowania stylów witryny.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-363">In this task, you will add a CSS stylesheet to define the styles of the site.</span></span>

1. <span data-ttu-id="4ed0b-364">Plik CSS i obrazy są zawarte w folderze **Source\Assets\Content** w tym laboratorium.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-364">The CSS file and images are included in the **Source\Assets\Content** folder of this Lab.</span></span> <span data-ttu-id="4ed0b-365">Aby dodać je do aplikacji, przeciągnij jej zawartość z okna **Eksploratora Windows** do **Eksplorator rozwiązań** w Visual Studio Express dla sieci Web, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="4ed0b-365">In order to add them to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Studio Express for Web, as shown below:</span></span>

    <span data-ttu-id="4ed0b-366">![Przeciąganie zawartości stylu](aspnet-mvc-4-fundamentals/_static/image12.png "Przeciąganie zawartości stylu")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-366">![Dragging style contents](aspnet-mvc-4-fundamentals/_static/image12.png "Dragging style contents")</span></span>

    <span data-ttu-id="4ed0b-367">*Przeciąganie zawartości stylu*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-367">*Dragging style contents*</span></span>
2. <span data-ttu-id="4ed0b-368">Zostanie wyświetlone okno dialogowe z ostrzeżeniem z prośbą o potwierdzenie zastąpienia pliku **site. css** i niektórych istniejących obrazów.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-368">A warning dialog will appear, asking for confirmation to replace **Site.css** file and some existing images.</span></span> <span data-ttu-id="4ed0b-369">Zaznacz pole wyboru **Zastosuj do wszystkich elementów** , a następnie kliknij przycisk **tak**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-369">Check **Apply to all items** and click **Yes**.</span></span>

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a><span data-ttu-id="4ed0b-370">Zadanie 3 — Dodawanie szablonu widoku</span><span class="sxs-lookup"><span data-stu-id="4ed0b-370">Task 3 - Adding a View Template</span></span>

<span data-ttu-id="4ed0b-371">W tym zadaniu dodasz szablon widoku w celu wygenerowania odpowiedzi HTML, która będzie używać strony wzorcowej układu i CSS dodanej w tym ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-371">In this task, you will add a View template to generate the HTML response that will use the layout master page and CSS added in this exercise.</span></span>

1. <span data-ttu-id="4ed0b-372">Aby użyć szablonu widoku podczas przeglądania strony głównej witryny, najpierw należy wskazać, że zamiast zwracać ciąg, Metoda **indeksu HomeController** zwróci **Widok**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-372">To use a View template when browsing the site's home page, you will first need to indicate that instead of returning a string, the **HomeController Index** method will return a **View**.</span></span> <span data-ttu-id="4ed0b-373">Otwórz klasę **HomeController** i zmień jej metodę **index** w taki sposób, aby zwracała **ActionResult**i zwracała **Widok ()** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-373">Open **HomeController** class and change its **Index** method to return an **ActionResult**, and have it return **View()**.</span></span>

    <span data-ttu-id="4ed0b-374">(Fragment kodu — *ASP.NET MVC 4 — podstawowe-EX4 HomeController index*)</span><span class="sxs-lookup"><span data-stu-id="4ed0b-374">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex4 HomeController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. <span data-ttu-id="4ed0b-375">Teraz musisz dodać odpowiedni szablon widoku.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-375">Now, you need to add an appropriate View template.</span></span> <span data-ttu-id="4ed0b-376">Aby to zrobić, **kliknij prawym przyciskiem myszy** wewnątrz metody akcji **indeksu** i wybierz polecenie **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-376">To do this, **right-click** inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="4ed0b-377">Spowoduje to wyświetlenie okna dialogowego **Dodawanie widoku** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-377">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="4ed0b-378">![Dodawanie widoku z poziomu metody index](aspnet-mvc-4-fundamentals/_static/image13.png "Dodawanie widoku z poziomu metody index")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-378">![Adding a View from within the Index method](aspnet-mvc-4-fundamentals/_static/image13.png "Adding a View from within the Index method")</span></span>

    <span data-ttu-id="4ed0b-379">*Dodawanie widoku z poziomu metody index*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-379">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="4ed0b-380">Zostanie wyświetlone okno dialogowe **Dodaj widok** , aby wygenerować plik szablonu widoku.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-380">The **Add View** Dialog will appear to generate a View template file.</span></span> <span data-ttu-id="4ed0b-381">Domyślnie to okno dialogowe wstępnie wypełnia nazwę szablonu widoku, aby była zgodna z metodą akcji, która będzie z niego korzystać.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-381">By default, this dialog pre-populates the name of the View template so that it matches the action method that will use it.</span></span> <span data-ttu-id="4ed0b-382">Ze względu na to, że w ramach metody akcji **index** w HomeController użyto menu kontekstowego **Dodaj** widok, w oknie dialogowym **Dodawanie widoku** znajduje się indeks jako domyślna nazwa widoku.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-382">Because you used the **Add View** context menu within the **Index** action method within the HomeController, the **Add View** dialog has Index as the default view name.</span></span> <span data-ttu-id="4ed0b-383">Kliknij pozycję **Add** (Dodaj).</span><span class="sxs-lookup"><span data-stu-id="4ed0b-383">Click **Add**.</span></span>

    <span data-ttu-id="4ed0b-384">![Okno dialogowe Dodawanie widoku](aspnet-mvc-4-fundamentals/_static/image14.png "Okno dialogowe Dodawanie widoku")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-384">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image14.png "Add View Dialog")</span></span>

    <span data-ttu-id="4ed0b-385">*Okno dialogowe Dodawanie widoku*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-385">*Add View Dialog*</span></span>
4. <span data-ttu-id="4ed0b-386">Program Visual Studio generuje szablon widoku **index. cshtml** w folderze **Views\Home** , a następnie go otwiera.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-386">Visual Studio generates an **Index.cshtml** view template inside the **Views\Home** folder and then opens it.</span></span>

    <span data-ttu-id="4ed0b-387">![Utworzono widok indeksu macierzystego](aspnet-mvc-4-fundamentals/_static/image15.png "Utworzono widok indeksu macierzystego")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-387">![Home Index view created](aspnet-mvc-4-fundamentals/_static/image15.png "Home Index view created")</span></span>

    <span data-ttu-id="4ed0b-388">*Utworzono widok indeksu macierzystego*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-388">*Home Index view created*</span></span>

    > [!NOTE]
    > <span data-ttu-id="4ed0b-389">Nazwa i lokalizacja pliku **index. cshtml** są odpowiednie i są zgodne z domyślnymi konwencjami nazewnictwa MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-389">name and location of the **Index.cshtml** file is relevant and follows the default ASP.NET MVC naming conventions.</span></span>
    > 
    > <span data-ttu-id="4ed0b-390">Folder \Views\**Home*\* jest zgodny z nazwą kontrolera (kontrolerem**głównym** ).</span><span class="sxs-lookup"><span data-stu-id="4ed0b-390">The folder \Views\**Home*\* matches the controller name (**Home** Controller).</span></span> <span data-ttu-id="4ed0b-391">Nazwa szablonu widoku (**indeks**) jest zgodna z metodą akcji kontrolera, która będzie wyświetlać widok.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-391">The View template name (**Index**), matches the controller action method which will be displaying the View.</span></span>
    > 
    > <span data-ttu-id="4ed0b-392">W ten sposób ASP.NET MVC unika konieczności jawnego określenia nazwy lub lokalizacji szablonu widoku w przypadku używania tej konwencji nazewnictwa do zwrócenia widoku.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-392">This way, ASP.NET MVC avoids having to explicitly specify the name or location of a View template when using this naming convention to return a View.</span></span>
5. <span data-ttu-id="4ed0b-393">Wygenerowany szablon widoku jest oparty na wcześniej zdefiniowanym szablonie **\_Layout. cshtml** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-393">The generated View template is based on the **\_layout.cshtml** template earlier defined.</span></span> <span data-ttu-id="4ed0b-394">Zaktualizuj Właściwość ViewBag. title do strony **głównej**i zmień jej główną zawartość na **stronę główną**, jak pokazano w poniższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="4ed0b-394">Update the ViewBag.Title property to **Home**, and change the main content to **This is the Home Page**, as shown in the code below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. <span data-ttu-id="4ed0b-395">Wybierz projekt **MvcMusicStore** w Eksplorator rozwiązań a następnie naciśnij klawisz **F5** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-395">Select **MvcMusicStore** project in the Solution Explorer and Press **F5** to run the Application.</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a><span data-ttu-id="4ed0b-396">Zadanie 4. Weryfikacja</span><span class="sxs-lookup"><span data-stu-id="4ed0b-396">Task 4: Verification</span></span>

<span data-ttu-id="4ed0b-397">Aby sprawdzić, czy wszystkie kroki w poprzednim ćwiczeniu zostały wykonane prawidłowo, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="4ed0b-397">In order to verify that you have correctly performed all the steps in the previous exercise, proceed as follows:</span></span>

<span data-ttu-id="4ed0b-398">W przypadku aplikacji otwartej w przeglądarce należy zauważyć, że:</span><span class="sxs-lookup"><span data-stu-id="4ed0b-398">With the application opened in a browser, you should note that:</span></span>

1. <span data-ttu-id="4ed0b-399">Metoda akcji indeksu HomeController znaleziona i wyświetlana w szablonie widoku **\Views\Home\Index.cshtml** , nawet jeśli kod nosi nazwę **Return ()** , ponieważ szablon widoku przestrzega standardowej konwencji nazewnictwa.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-399">The HomeController's Index action method found and displayed the **\Views\Home\Index.cshtml** View template, even though the code called **return View()**, because the View template followed the standard naming convention.</span></span>
2. <span data-ttu-id="4ed0b-400">Na stronie głównej zostanie wyświetlony komunikat powitalny zdefiniowany w szablonie widoku **\Views\Home\Index.cshtml** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-400">The Home Page displays the welcome message defined within the **\Views\Home\Index.cshtml** view template.</span></span>
3. <span data-ttu-id="4ed0b-401">Na stronie głównej jest używany szablon **\_Layout. cshtml** , dlatego Komunikat powitalny jest zawarty w układzie HTML witryny standardowej.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-401">The Home Page is using the **\_layout.cshtml** template, and so the welcome message is contained within the standard site HTML layout.</span></span>

    <span data-ttu-id="4ed0b-402">![Widok indeksu macierzystego przy użyciu zdefiniowanego LayoutPage i stylu](aspnet-mvc-4-fundamentals/_static/image16.png "Widok indeksu macierzystego przy użyciu zdefiniowanego LayoutPage i stylu")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-402">![Home Index View using the defined LayoutPage and style](aspnet-mvc-4-fundamentals/_static/image16.png "Home Index View using the defined LayoutPage and style")</span></span>

    <span data-ttu-id="4ed0b-403">*Widok indeksu macierzystego przy użyciu zdefiniowanego LayoutPage i stylu*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-403">*Home Index View using the defined LayoutPage and style*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a><span data-ttu-id="4ed0b-404">Ćwiczenie 5. Tworzenie modelu widoku</span><span class="sxs-lookup"><span data-stu-id="4ed0b-404">Exercise 5: Creating a View Model</span></span>

<span data-ttu-id="4ed0b-405">Do tej pory Twoje widoki wyświetlają kod HTML stałe, ale w celu utworzenia dynamicznych aplikacji sieci Web szablon widoku powinien otrzymywać informacje z kontrolera.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-405">So far, you made your Views display hardcoded HTML, but, in order to create dynamic web applications, the View template should receive information from the Controller.</span></span> <span data-ttu-id="4ed0b-406">Jedną z typowych technik do użycia w tym celu jest wzorzec **ViewModel** , który umożliwia kontrolerowi spakowanie wszystkich informacji potrzebnych do wygenerowania odpowiedniej odpowiedzi html.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-406">One common technique to be used for that purpose is the **ViewModel** pattern, which allows a Controller to package up all the information needed to generate the appropriate HTML response.</span></span>

<span data-ttu-id="4ed0b-407">W tym ćwiczeniu dowiesz się, jak utworzyć klasę ViewModel i dodać wymagane właściwości: liczbę gatunków w sklepie i listę tych gatunków.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-407">In this exercise, you will learn how to create a ViewModel class and add the required properties: the number of genres in the store and a list of those genres.</span></span> <span data-ttu-id="4ed0b-408">Można również zaktualizować StoreController, aby korzystał z utworzonych ViewModel, a wreszcie utworzyć nowy szablon widoku, który będzie wyświetlał wymienione właściwości na stronie.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-408">You will also update the StoreController to use the created ViewModel, and finally, you will create a new View template that will display the mentioned properties in the page.</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a><span data-ttu-id="4ed0b-409">Zadanie 1 — Tworzenie klasy ViewModel</span><span class="sxs-lookup"><span data-stu-id="4ed0b-409">Task 1 - Creating a ViewModel Class</span></span>

<span data-ttu-id="4ed0b-410">W tym zadaniu utworzysz klasę ViewModel, która będzie implementować scenariusz tworzenia gatunku dla danego sklepu.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-410">In this task, you will create a ViewModel class that will implement the Store genre listing scenario.</span></span>

1. <span data-ttu-id="4ed0b-411">Jeśli nie jest jeszcze otwarty, uruchom **vs Express for Web**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-411">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="4ed0b-412">W menu **plik** wybierz **Otwórz projekt**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-412">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="4ed0b-413">W oknie dialogowym Otwórz projekt przejdź do **Source\Ex05-CreatingAViewModel\Begin**, wybierz pozycję **Rozpocznij. sln** , a następnie kliknij pozycję **Otwórz**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-413">In the Open Project dialog, browse to **Source\Ex05-CreatingAViewModel\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="4ed0b-414">Alternatywnie można nadal korzystać z rozwiązania uzyskanego po zakończeniu poprzedniego ćwiczenia.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-414">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="4ed0b-415">Jeśli otwarto **udostępnione rozwiązanie** , należy pobrać brakujące pakiety NuGet przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-415">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="4ed0b-416">W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-416">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="4ed0b-417">W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-417">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="4ed0b-418">Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-418">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="4ed0b-419">Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-419">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="4ed0b-420">Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-420">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="4ed0b-421">To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-421">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="4ed0b-422">Utwórz folder **modele widoków** , aby pomieścić ViewModel.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-422">Create a **ViewModels** folder to hold the ViewModel.</span></span> <span data-ttu-id="4ed0b-423">W tym celu kliknij prawym przyciskiem myszy projekt **MvcMusicStore** najwyższego poziomu, wybierz polecenie **Dodaj** , a następnie pozycję **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-423">To do this, right-click the top-level **MvcMusicStore** project, select **Add** and then **New Folder**.</span></span>

    <span data-ttu-id="4ed0b-424">![Dodawanie nowego folderu](aspnet-mvc-4-fundamentals/_static/image17.png "Dodawanie nowego folderu")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-424">![Adding a new folder](aspnet-mvc-4-fundamentals/_static/image17.png "Adding a new folder")</span></span>

    <span data-ttu-id="4ed0b-425">*Dodawanie nowego folderu*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-425">*Adding a new folder*</span></span>
4. <span data-ttu-id="4ed0b-426">Nazwij folder *modele widoków*.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-426">Name the folder *ViewModels*.</span></span>

    <span data-ttu-id="4ed0b-427">![Folder modele widoków w Eksplorator rozwiązań](aspnet-mvc-4-fundamentals/_static/image18.png "Folder modele widoków w Eksplorator rozwiązań")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-427">![ViewModels folder in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image18.png "ViewModels folder in Solution Explorer")</span></span>

    <span data-ttu-id="4ed0b-428">*Folder modele widoków w Eksplorator rozwiązań*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-428">*ViewModels folder in Solution Explorer*</span></span>
5. <span data-ttu-id="4ed0b-429">Utwórz klasę **ViewModel** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-429">Create a **ViewModel** class.</span></span> <span data-ttu-id="4ed0b-430">Aby to zrobić, kliknij prawym przyciskiem myszy folder **modele widoków** , a następnie wybierz pozycję **Dodaj** , a następnie pozycję **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-430">To do this, right-click on the **ViewModels** folder recently created, select **Add** and then **New Item**.</span></span> <span data-ttu-id="4ed0b-431">W obszarze **kod**wybierz element **klasy** i Nazwij plik *StoreIndexViewModel.cs*, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-431">Under **Code**, choose the **Class** item and name the file *StoreIndexViewModel.cs*, then click **Add**.</span></span>

    <span data-ttu-id="4ed0b-432">![Dodawanie nowej klasy](aspnet-mvc-4-fundamentals/_static/image19.png "Dodawanie nowej klasy")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-432">![Adding a new Class](aspnet-mvc-4-fundamentals/_static/image19.png "Adding a new Class")</span></span>

    <span data-ttu-id="4ed0b-433">*Dodawanie nowej klasy*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-433">*Adding a new Class*</span></span>

    <span data-ttu-id="4ed0b-434">![Tworzenie klasy StoreIndexViewModel](aspnet-mvc-4-fundamentals/_static/image20.png "Tworzenie klasy StoreIndexViewModel")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-434">![Creating StoreIndexViewModel class](aspnet-mvc-4-fundamentals/_static/image20.png "Creating StoreIndexViewModel class")</span></span>

    <span data-ttu-id="4ed0b-435">*Tworzenie klasy StoreIndexViewModel*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-435">*Creating StoreIndexViewModel class*</span></span>

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a><span data-ttu-id="4ed0b-436">Zadanie 2 — Dodawanie właściwości do klasy ViewModel</span><span class="sxs-lookup"><span data-stu-id="4ed0b-436">Task 2 - Adding Properties to the ViewModel class</span></span>

<span data-ttu-id="4ed0b-437">Istnieją dwa parametry, które mają zostać przesłane z StoreController do szablonu widoku, aby wygenerować oczekiwaną odpowiedź HTML: liczbę gatunków w sklepie i listę tych gatunków.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-437">There are two parameters to be passed from the StoreController to the View template in order to generate the expected HTML response: the number of genres in the store and a list of those genres.</span></span>

<span data-ttu-id="4ed0b-438">W tym zadaniu dodasz te 2 właściwości do klasy **StoreIndexViewModel** : **NumberOfGenres** (Integer) i **gatunek** (lista ciągów).</span><span class="sxs-lookup"><span data-stu-id="4ed0b-438">In this task, you will add those 2 properties to the **StoreIndexViewModel** class: **NumberOfGenres** (an integer) and **Genres** (a list of strings).</span></span>

1. <span data-ttu-id="4ed0b-439">Dodaj właściwości **NumberOfGenres** i **gatuneks** do klasy **StoreIndexViewModel** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-439">Add **NumberOfGenres** and **Genres** properties to the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="4ed0b-440">Aby to zrobić, Dodaj następujące 2 wiersze do definicji klasy:</span><span class="sxs-lookup"><span data-stu-id="4ed0b-440">To do this, add the following 2 lines to the class definition:</span></span>

    <span data-ttu-id="4ed0b-441">(Fragment kodu- *ASP.NET MVC 4 podstawowe-Ex5 StoreIndexViewModel Properties*)</span><span class="sxs-lookup"><span data-stu-id="4ed0b-441">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel properties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

> [!NOTE]
> <span data-ttu-id="4ed0b-442">W notacji **{get; set;}** jest używana C#funkcja właściwości, która została zaimplementowana.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-442">The **{ get; set; }** notation makes use of C#'s auto-implemented properties feature.</span></span> <span data-ttu-id="4ed0b-443">Zapewnia korzyści wynikające z właściwości bez konieczności deklarowania pola zapasowego.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-443">It provides the benefits of a property without requiring us to declare a backing field.</span></span>

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a><span data-ttu-id="4ed0b-444">Zadanie 3 — Aktualizowanie StoreController do korzystania z StoreIndexViewModel</span><span class="sxs-lookup"><span data-stu-id="4ed0b-444">Task 3 - Updating StoreController to use the StoreIndexViewModel</span></span>

<span data-ttu-id="4ed0b-445">Klasa **StoreIndexViewModel** hermetyzuje informacje, które są konieczne do przekazania z metody **indeksu** **StoreController**do szablonu widoku w celu wygenerowania odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-445">The **StoreIndexViewModel** class encapsulates the information needed to pass from **StoreController**'s **Index** method to a View template in order to generate a response.</span></span>

<span data-ttu-id="4ed0b-446">W tym zadaniu zaktualizujesz **StoreController** , aby użyć **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-446">In this task, you will update the **StoreController** to use the **StoreIndexViewModel**.</span></span>

1. <span data-ttu-id="4ed0b-447">Otwórz klasę **StoreController** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-447">Open **StoreController** class.</span></span>

    <span data-ttu-id="4ed0b-448">![Otwieranie klasy StoreController](aspnet-mvc-4-fundamentals/_static/image21.png "Otwieranie klasy StoreController")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-448">![Opening StoreController class](aspnet-mvc-4-fundamentals/_static/image21.png "Opening StoreController class")</span></span>

    <span data-ttu-id="4ed0b-449">*Otwieranie klasy StoreController*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-449">*Opening StoreController class*</span></span>
2. <span data-ttu-id="4ed0b-450">Aby można było użyć klasy **StoreIndexViewModel** z **StoreController**, Dodaj następującą przestrzeń nazw u góry kodu **StoreController** :</span><span class="sxs-lookup"><span data-stu-id="4ed0b-450">In order to use the **StoreIndexViewModel** class from the **StoreController**, add the following namespace at the top of the **StoreController** code:</span></span>

    <span data-ttu-id="4ed0b-451">(Fragment kodu- *ASP.NET MVC 4 — podstawowe-Ex5 StoreIndexViewModel za pomocą modele widoków*)</span><span class="sxs-lookup"><span data-stu-id="4ed0b-451">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel using ViewModels*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. <span data-ttu-id="4ed0b-452">Zmień metodę akcji **index** **StoreController**tak, aby tworzyła i wypełnia obiekt **StoreIndexViewModel** , a następnie przekazuje go do szablonu widoku w celu wygenerowania odpowiedzi html.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-452">Change the **StoreController**'s **Index** action method so that it creates and populates a **StoreIndexViewModel** object and then passes it off to a View template to generate an HTML response with it.</span></span>

    > [!NOTE]
    > <span data-ttu-id="4ed0b-453">W programie Lab &quot;ASP.NET modele MVC i dostęp do danych&quot; napiszesz kod, który pobiera listę gatunków sklepu z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-453">In Lab &quot;ASP.NET MVC Models and Data Access&quot; you will write code that retrieves the list of store genres from a database.</span></span> <span data-ttu-id="4ed0b-454">W poniższym kodzie zostanie utworzona **Lista** fikcyjnych gatunków danych, które zapełnią **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-454">In the following code, you will create a **List** of dummy data genres that will populate the **StoreIndexViewModel**.</span></span>
    > 
    > <span data-ttu-id="4ed0b-455">Po utworzeniu i skonfigurowaniu obiektu **StoreIndexViewModel** zostanie on przesłany jako argument do metody **widoku** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-455">After creating and setting up the **StoreIndexViewModel** object, it will be passed as an argument to the **View** method.</span></span> <span data-ttu-id="4ed0b-456">Oznacza to, że szablon widoku będzie używać tego obiektu do generowania odpowiedzi HTML.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-456">This indicates that the View template will use that object to generate an HTML response with it.</span></span>
4. <span data-ttu-id="4ed0b-457">Zastąp metodę **index** następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="4ed0b-457">Replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="4ed0b-458">(Fragment kodu- *ASP.NET MVC 4 podstawowe-Ex5 StoreController index*)</span><span class="sxs-lookup"><span data-stu-id="4ed0b-458">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreController Index method*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

> [!NOTE]
> <span data-ttu-id="4ed0b-459">Jeśli nie znasz programu C#, możesz założyć, że użycie funkcji **var** oznacza, że zmienna **viewModel** jest opóźniona.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-459">If you're unfamiliar with C#, you may assume that using **var** means that the **viewModel** variable is late-bound.</span></span> <span data-ttu-id="4ed0b-460">To nie jest poprawne — C# kompilator używa wnioskowania typu w oparciu o przypisane elementy do zmiennej, aby określić, że **ViewModel** jest typu **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-460">That's not correct - the C# compiler is using type-inference based on what you assign to the variable to determine that **viewModel** is of type **StoreIndexViewModel**.</span></span> <span data-ttu-id="4ed0b-461">Ponadto przez skompilowanie lokalnej zmiennej **viewModel** jako typu **StoreIndexViewModel** uzyskasz sprawdzanie czasu kompilowania i obsługę edytora kodu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-461">Also, by compiling the local **viewModel** variable as a **StoreIndexViewModel** type you get compile-time checking and Visual Studio code-editor support.</span></span>

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a><span data-ttu-id="4ed0b-462">Zadanie 4 — Tworzenie szablonu widoku korzystającego z StoreIndexViewModel</span><span class="sxs-lookup"><span data-stu-id="4ed0b-462">Task 4 - Creating a View Template that Uses StoreIndexViewModel</span></span>

<span data-ttu-id="4ed0b-463">W tym zadaniu utworzysz szablon widoku, który będzie używać obiektu StoreIndexViewModel przekazaną przez kontroler do wyświetlania listy gatunków.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-463">In this task, you will create a View template that will use a StoreIndexViewModel object passed from the Controller to display a list of genres.</span></span>

1. <span data-ttu-id="4ed0b-464">Przed utworzeniem nowego szablonu widoku skompilujemy projekt, aby **okno dialogowe Dodawanie widoku** wie o klasie **StoreIndexViewModel** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-464">Before creating the new View template, let's build the project so that the **Add View Dialog** knows about the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="4ed0b-465">Skompiluj projekt, wybierając element menu **kompilacja** , a następnie **Skompiluj MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-465">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="4ed0b-466">![Kompilowanie projektu](aspnet-mvc-4-fundamentals/_static/image22.png "Kompilowanie projektu")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-466">![Building the project](aspnet-mvc-4-fundamentals/_static/image22.png "Building the project")</span></span>

    <span data-ttu-id="4ed0b-467">*Kompilowanie projektu*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-467">*Building the project*</span></span>
2. <span data-ttu-id="4ed0b-468">Utwórz nowy szablon widoku.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-468">Create a new View template.</span></span> <span data-ttu-id="4ed0b-469">Aby to zrobić, kliknij prawym przyciskiem myszy wewnątrz metody **index** i wybierz polecenie **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-469">To do that, right-click inside the **Index** method and select **Add View**.</span></span>

    <span data-ttu-id="4ed0b-470">![Dodawanie widoku](aspnet-mvc-4-fundamentals/_static/image23.png "Dodawanie widoku")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-470">![Adding a View](aspnet-mvc-4-fundamentals/_static/image23.png "Adding a View")</span></span>

    <span data-ttu-id="4ed0b-471">*Dodawanie widoku*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-471">*Adding a View*</span></span>
3. <span data-ttu-id="4ed0b-472">Ponieważ **okno dialogowe Dodawanie widoku** zostało wywołane z **StoreController**, spowoduje to dodanie domyślnego szablonu widoku w pliku **\Views\Store\Index.cshtml** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-472">Because the **Add View Dialog** was invoked from the **StoreController**, it will add the View template by default in a **\Views\Store\Index.cshtml** file.</span></span> <span data-ttu-id="4ed0b-473">Zaznacz pole wyboru **Utwórz silnie wpisaną-View** , a następnie wybierz **StoreIndexViewModel** jako **klasę modelu**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-473">Check the **Create a strongly-typed-view** checkbox and then select **StoreIndexViewModel** as the **Model class**.</span></span> <span data-ttu-id="4ed0b-474">Upewnij się również, że wybrany aparat widoku to **Razor**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-474">Also, make sure that the View engine selected is **Razor**.</span></span> <span data-ttu-id="4ed0b-475">Kliknij pozycję **Add** (Dodaj).</span><span class="sxs-lookup"><span data-stu-id="4ed0b-475">Click **Add**.</span></span>

    <span data-ttu-id="4ed0b-476">![Okno dialogowe Dodawanie widoku](aspnet-mvc-4-fundamentals/_static/image24.png "Okno dialogowe Dodawanie widoku")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-476">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image24.png "Add View Dialog")</span></span>

    <span data-ttu-id="4ed0b-477">*Okno dialogowe Dodawanie widoku*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-477">*Add View Dialog*</span></span>

    <span data-ttu-id="4ed0b-478">Plik szablonu widoku **\Views\Store\Index.cshtml** zostanie utworzony i otwarty.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-478">The **\Views\Store\Index.cshtml** View template file is created and opened.</span></span> <span data-ttu-id="4ed0b-479">W oparciu o informacje podane do okna dialogowego **Dodawanie widoku** w ostatnim kroku, szablon widoku będzie oczekiwać wystąpienia **StoreIndexViewModel** jako danych do użycia w celu wygenerowania odpowiedzi html.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-479">Based on the information provided to the **Add View** dialog in the last step, the View template will expect a **StoreIndexViewModel** instance as the data to use to generate an HTML response.</span></span> <span data-ttu-id="4ed0b-480">Zobaczysz, że szablon dziedziczy `ViewPage<musicstore.viewmodels.storeindexviewmodel>` w C#elemencie.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-480">You will notice that the template inherits a `ViewPage<musicstore.viewmodels.storeindexviewmodel>` in C#.</span></span>

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a><span data-ttu-id="4ed0b-481">Zadanie 5 — Aktualizowanie szablonu widoku</span><span class="sxs-lookup"><span data-stu-id="4ed0b-481">Task 5 - Updating the View Template</span></span>

<span data-ttu-id="4ed0b-482">W tym zadaniu zostanie zaktualizowany szablon widoku utworzony w ostatnim zadaniu w celu pobrania liczby gatunków i ich nazw na stronie.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-482">In this task, you will update the View template created in the last task to retrieve the number of genres and their names within the page.</span></span>

> [!NOTE]
> <span data-ttu-id="4ed0b-483">Użyjesz @ Syntax (często nazywanej &quot;kodem Nuggets&quot;), aby wykonać kod w szablonie widoku.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-483">You will use @ syntax (often referred to as &quot;code nuggets&quot;) to execute code within the View template.</span></span>

1. <span data-ttu-id="4ed0b-484">W pliku **index. cshtml** w folderze **Store** Zamień swój kod na następujący:</span><span class="sxs-lookup"><span data-stu-id="4ed0b-484">In the **Index.cshtml** file, within the **Store** folder, replace its code with the following:</span></span>

[!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

    > [!NOTE]
    > As soon as you finish typing the period after the word **Model**, Visual Studio's Intellisense will show a list of possible properties and methods to choose from.
    > 
    > ![](aspnet-mvc-4-fundamentals/_static/image25.png)
    > 
    > *Getting Model properties and methods with Visual Studio's IntelliSense*
    > 
    > The **Model** property references the **StoreIndexViewModel** object that the Controller passed to the View template. This means that you can access all of the data passed from the Controller to the View template via the **Model** property, and format it into an appropriate HTML response within the View template.
    > 
    > You can just select the **NumberOfGenres** property from the Intellisense list rather than typing it in and then it will auto-complete it by pressing the **tab key**.
2. <span data-ttu-id="4ed0b-485">Pętla nad listą gatunek w **StoreIndexViewModel** i Utwórz kod HTML **&lt;ul&gt;** list przy użyciu pętli **foreach** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-485">Loop over the genre list in **StoreIndexViewModel** and create an HTML **&lt;ul&gt;** list using a **foreach** loop.</span></span>
   <span data-ttu-id="4ed0b-486">(C#)</span><span class="sxs-lookup"><span data-stu-id="4ed0b-486">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. <span data-ttu-id="4ed0b-487">Naciśnij klawisz **F5** , aby uruchomić aplikację i przeglądać **/Store**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-487">Press **F5** to run the Application and browse **/Store**.</span></span> <span data-ttu-id="4ed0b-488">Zostanie wyświetlona lista gatunków przeniesiona w obiekcie **StoreIndexViewModel** z **StoreController** do szablonu widoku.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-488">You will see the list of genres passed in the **StoreIndexViewModel** object from the **StoreController** to the View template.</span></span>

    <span data-ttu-id="4ed0b-489">![Widok wyświetlania listy gatunków](aspnet-mvc-4-fundamentals/_static/image26.png "Widok wyświetlania listy gatunków")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-489">![View displaying a list of genres](aspnet-mvc-4-fundamentals/_static/image26.png "View displaying a list of genres")</span></span>

    <span data-ttu-id="4ed0b-490">*Widok wyświetlania listy gatunków*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-490">*View displaying a list of genres*</span></span>
4. <span data-ttu-id="4ed0b-491">Zamknij okno przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-491">Close the browser.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a><span data-ttu-id="4ed0b-492">Ćwiczenie 6: Używanie parametrów w widoku</span><span class="sxs-lookup"><span data-stu-id="4ed0b-492">Exercise 6: Using Parameters in View</span></span>

<span data-ttu-id="4ed0b-493">W ćwiczeniu 3 przedstawiono sposób przekazywania parametrów do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-493">In Exercise 3 you learned how to pass parameters to the Controller.</span></span> <span data-ttu-id="4ed0b-494">W tym ćwiczeniu dowiesz się, jak używać tych parametrów w szablonie widoku.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-494">In this exercise, you will learn how to use those parameters in the View template.</span></span> <span data-ttu-id="4ed0b-495">W tym celu należy najpierw wprowadzić klasy modelu, które ułatwią zarządzanie danymi i logiką domeny.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-495">For that purpose, you will be introduced first to Model classes that will help you to manage your data and domain logic.</span></span> <span data-ttu-id="4ed0b-496">Ponadto dowiesz się, jak tworzyć linki do stron w aplikacji ASP.NET MVC, bez obaw, takich jak kodowanie ścieżek adresów URL.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-496">Additionally, you will learn how to create links to pages inside the ASP.NET MVC application without worrying of things like URL paths encoding.</span></span>

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a><span data-ttu-id="4ed0b-497">Zadanie 1 — Dodawanie klas modelu</span><span class="sxs-lookup"><span data-stu-id="4ed0b-497">Task 1 - Adding Model Classes</span></span>

<span data-ttu-id="4ed0b-498">W przeciwieństwie do modele widoków, które są tworzone tylko w celu przekazywania informacji z kontrolera do widoku, klasy modelu są kompilowane tak, aby zawierały dane i logikę domen i zarządzać nimi.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-498">Unlike ViewModels, which are created just to pass information from the Controller to the View, Model classes are built to contain and manage data and domain logic.</span></span> <span data-ttu-id="4ed0b-499">W tym zadaniu zostaną dodane dwie klasy modelu do reprezentowania tych koncepcji: **gatunek** i **album**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-499">In this task you will add two model classes to represent these concepts: **Genre** and **Album**.</span></span>

1. <span data-ttu-id="4ed0b-500">Jeśli nie jest jeszcze otwarty, uruchom **vs Express for Web**</span><span class="sxs-lookup"><span data-stu-id="4ed0b-500">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="4ed0b-501">W menu **plik** wybierz **Otwórz projekt**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-501">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="4ed0b-502">W oknie dialogowym Otwórz projekt przejdź do **Source\Ex06-UsingParametersInView\Begin**, wybierz pozycję **Rozpocznij. sln** , a następnie kliknij pozycję **Otwórz**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-502">In the Open Project dialog, browse to **Source\Ex06-UsingParametersInView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="4ed0b-503">Alternatywnie można nadal korzystać z rozwiązania uzyskanego po zakończeniu poprzedniego ćwiczenia.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-503">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="4ed0b-504">Jeśli otwarto **udostępnione rozwiązanie** , należy pobrać brakujące pakiety NuGet przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-504">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="4ed0b-505">W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-505">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="4ed0b-506">W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-506">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="4ed0b-507">Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-507">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="4ed0b-508">Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-508">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="4ed0b-509">Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-509">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="4ed0b-510">To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-510">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="4ed0b-511">Dodaj klasę modelu **gatunek** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-511">Add a **Genre** Model class.</span></span> <span data-ttu-id="4ed0b-512">W tym celu kliknij prawym przyciskiem myszy folder **modele** w **Eksplorator rozwiązań**, wybierz polecenie **Dodaj** , a następnie opcję **nowy element** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-512">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="4ed0b-513">W obszarze **kod**wybierz element **klasy** i Nazwij plik *Genre.cs*, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-513">Under **Code**, choose the **Class** item and name the file *Genre.cs*, then click **Add**.</span></span>

    <span data-ttu-id="4ed0b-514">![Dodawanie klasy](aspnet-mvc-4-fundamentals/_static/image27.png "Dodawanie klasy")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-514">![Adding a class](aspnet-mvc-4-fundamentals/_static/image27.png "Adding a class")</span></span>

    <span data-ttu-id="4ed0b-515">*Dodawanie nowego elementu*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-515">*Adding a new item*</span></span>

    <span data-ttu-id="4ed0b-516">![Dodaj klasę modelu gatunku](aspnet-mvc-4-fundamentals/_static/image28.png "Dodaj klasę modelu gatunku")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-516">![Add Genre Model Class](aspnet-mvc-4-fundamentals/_static/image28.png "Add Genre Model Class")</span></span>

    <span data-ttu-id="4ed0b-517">*Dodaj klasę modelu gatunku*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-517">*Add Genre Model Class*</span></span>
4. <span data-ttu-id="4ed0b-518">Dodaj właściwość **name** do klasy gatunek.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-518">Add a **Name** property to the Genre class.</span></span> <span data-ttu-id="4ed0b-519">Aby to zrobić, Dodaj następujący kod:</span><span class="sxs-lookup"><span data-stu-id="4ed0b-519">To do this, add the following code:</span></span>

    <span data-ttu-id="4ed0b-520">(Fragment kodu- *ASP.NET MVC 4 podstawy-Ex6 gatunek*)</span><span class="sxs-lookup"><span data-stu-id="4ed0b-520">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Genre*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. <span data-ttu-id="4ed0b-521">Postępując zgodnie z tą samą procedurą jak wcześniej, Dodaj klasę **albumu** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-521">Following the same procedure as before, add an **Album** class.</span></span> <span data-ttu-id="4ed0b-522">W tym celu kliknij prawym przyciskiem myszy folder **modele** w **Eksplorator rozwiązań**, wybierz polecenie **Dodaj** , a następnie opcję **nowy element** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-522">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="4ed0b-523">W obszarze **kod**wybierz element **klasy** i Nazwij plik *album.cs*, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-523">Under **Code**, choose the **Class** item and name the file *Album.cs*, then click **Add**.</span></span>
6. <span data-ttu-id="4ed0b-524">Dodaj dwie właściwości do klasy albumu: **gatunek** i **tytuł**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-524">Add two properties to the Album class: **Genre** and **Title**.</span></span> <span data-ttu-id="4ed0b-525">Aby to zrobić, Dodaj następujący kod:</span><span class="sxs-lookup"><span data-stu-id="4ed0b-525">To do this, add the following code:</span></span>

    <span data-ttu-id="4ed0b-526">(Fragment kodu — *ASP.NET MVC 4 — podstawy — Ex6 album*)</span><span class="sxs-lookup"><span data-stu-id="4ed0b-526">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Album*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a><span data-ttu-id="4ed0b-527">Zadanie 2 — Dodawanie elementu StoreBrowseViewModel</span><span class="sxs-lookup"><span data-stu-id="4ed0b-527">Task 2 - Adding a StoreBrowseViewModel</span></span>

<span data-ttu-id="4ed0b-528">W tym zadaniu zostanie użyta **StoreBrowseViewModel** , aby wyświetlić albumy pasujące do wybranego gatunku.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-528">A **StoreBrowseViewModel** will be used in this task to show the Albums that match a selected Genre.</span></span> <span data-ttu-id="4ed0b-529">W tym zadaniu utworzysz tę klasę, a następnie dodasz dwie właściwości, aby obsłużyć **gatunek** i jego listę **albumów**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-529">In this task, you will create this class and then add two properties to handle the **Genre** and its **Album**'s List.</span></span>

1. <span data-ttu-id="4ed0b-530">Dodaj klasę **StoreBrowseViewModel** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-530">Add a **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="4ed0b-531">W tym celu kliknij prawym przyciskiem myszy folder **modele widoków** w **Eksplorator rozwiązań**, wybierz pozycję **Dodaj** , a następnie opcję **nowy element** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-531">To do this, right-click the **ViewModels** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="4ed0b-532">W obszarze **kod**wybierz element **klasy** i Nazwij plik *StoreBrowseViewModel.cs*, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-532">Under **Code**, choose the **Class** item and name the file *StoreBrowseViewModel.cs*, then click **Add**.</span></span>
2. <span data-ttu-id="4ed0b-533">Dodaj odwołanie do modeli w klasie **StoreBrowseViewModel** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-533">Add a reference to the Models in **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="4ed0b-534">Aby to zrobić, Dodaj następujące polecenie przy użyciu przestrzeni nazw:</span><span class="sxs-lookup"><span data-stu-id="4ed0b-534">To do this, add the following using namespace:</span></span>

    <span data-ttu-id="4ed0b-535">(Fragment kodu — *ASP.NET MVC 4 — podstawowe-Ex6 UsingModel*)</span><span class="sxs-lookup"><span data-stu-id="4ed0b-535">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModel*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. <span data-ttu-id="4ed0b-536">Dodaj dwie właściwości do klasy **StoreBrowseViewModel** : **gatunek** i **albumy**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-536">Add two properties to **StoreBrowseViewModel** class: **Genre** and **Albums**.</span></span> <span data-ttu-id="4ed0b-537">Aby to zrobić, Dodaj następujący kod:</span><span class="sxs-lookup"><span data-stu-id="4ed0b-537">To do this, add the following code:</span></span>

    <span data-ttu-id="4ed0b-538">(Fragment kodu — *ASP.NET MVC 4 — podstawowe-Ex6 ModelProperties*)</span><span class="sxs-lookup"><span data-stu-id="4ed0b-538">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 ModelProperties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

> [!NOTE]
> <span data-ttu-id="4ed0b-539">Co to **jest lista&lt;&gt;albumu** ?: w tej definicji jest **używana lista&lt;t&gt;** typ, gdzie **t** ogranicza typ, do którego należą elementy tej **listy** , w tym przypadku **album** (lub dowolne z jego obiektów podrzędnych).</span><span class="sxs-lookup"><span data-stu-id="4ed0b-539">What is **List&lt;Album&gt;** ?: This definition is using the **List&lt;T&gt;** type, where **T** constrains the type to which elements of this **List** belong to, in this case **Album** (or any of its descendants).</span></span>
> 
> <span data-ttu-id="4ed0b-540">Możliwość projektowania klas i metod, które opóźniają specyfikację jednego lub więcej typów do momentu zadeklarowania klasy lub metody i wystąpienia przez kod klienta jest funkcją C# języka o nazwie **Generics**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-540">This ability to design classes and methods that defer the specification of one or more types until the class or method is declared and instantiated by client code is a feature of the C# language called **Generics**.</span></span>
> 
> <span data-ttu-id="4ed0b-541">**Lista&lt;t&gt;** jest ogólnym odpowiednikiem typu **ArrayList** i jest dostępna w przestrzeni nazw **System. Collections. Generic** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-541">**List&lt;T&gt;** is the generic equivalent of the **ArrayList** type and is available in the **System.Collections.Generic** namespace.</span></span> <span data-ttu-id="4ed0b-542">Jedną z korzyści wynikających z używania typów **ogólnych** jest to, że od momentu określenia typu, nie trzeba zadbać o wykonywanie operacji sprawdzania typu, takich jak Rzutowanie elementów do **albumu** , tak jak w przypadku **ArrayList**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-542">One of the benefits of using **generics** is that since the type is specified, you do not need to take care of type checking operations such as casting the elements into **Album** as you would do with an **ArrayList**.</span></span>

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a><span data-ttu-id="4ed0b-543">Zadanie 3 — Używanie nowego ViewModel w StoreController</span><span class="sxs-lookup"><span data-stu-id="4ed0b-543">Task 3 - Using the New ViewModel in the StoreController</span></span>

<span data-ttu-id="4ed0b-544">W tym zadaniu zmodyfikujemy metody akcji **przeglądania** i **szczegółów** **StoreController**, aby użyć nowej **StoreBrowseViewModel**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-544">In this task, you will modify the **StoreController**'s **Browse** and **Details** action methods to use the new **StoreBrowseViewModel**.</span></span>

1. <span data-ttu-id="4ed0b-545">Dodaj odwołanie do folderu models w klasie **StoreController** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-545">Add a reference to the Models folder in **StoreController** class.</span></span> <span data-ttu-id="4ed0b-546">W tym celu rozwiń folder **controllers** w **Eksplorator rozwiązań** i Otwórz klasę **StoreController** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-546">To do this, expand the **Controllers** folder in the **Solution Explorer** and open the **StoreController** class.</span></span> <span data-ttu-id="4ed0b-547">Następnie Dodaj następujący kod:</span><span class="sxs-lookup"><span data-stu-id="4ed0b-547">Then add the following code:</span></span>

    <span data-ttu-id="4ed0b-548">(Fragment kodu — *ASP.NET MVC 4 — podstawowe-Ex6 UsingModelInController*)</span><span class="sxs-lookup"><span data-stu-id="4ed0b-548">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModelInController*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. <span data-ttu-id="4ed0b-549">Zastąp metodę **przeglądania** akcją, aby użyć klasy **StoreViewBrowseController** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-549">Replace the **Browse** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="4ed0b-550">Utworzysz gatunek i dwa nowe obiekty albumów z fikcyjnymi danymi (w następnym ćwiczeniu laboratorium będziesz używać rzeczywistych danych z bazy danych).</span><span class="sxs-lookup"><span data-stu-id="4ed0b-550">You will create a Genre and two new Albums objects with dummy data (in the next Hands-on Lab you will consume real data from a database).</span></span> <span data-ttu-id="4ed0b-551">Aby to zrobić, Zastąp metodę **Browse** następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="4ed0b-551">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="4ed0b-552">(Fragment kodu — *ASP.NET MVC 4 — podstawowe-Ex6 BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="4ed0b-552">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. <span data-ttu-id="4ed0b-553">Zastąp metodę akcji **Details** , aby użyć klasy **StoreViewBrowseController** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-553">Replace the **Details** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="4ed0b-554">Utworzysz nowy obiekt **albumu** do zwrócenia do **widoku**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-554">You will create a new **Album** object to be returned to the **View**.</span></span> <span data-ttu-id="4ed0b-555">Aby to zrobić, Zastąp metodę **Details** następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="4ed0b-555">To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="4ed0b-556">(Fragment kodu — *ASP.NET MVC 4 — podstawowe-Ex6 DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="4ed0b-556">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a><span data-ttu-id="4ed0b-557">Zadanie 4 — Dodawanie szablonu widoku przeglądania</span><span class="sxs-lookup"><span data-stu-id="4ed0b-557">Task 4 - Adding a Browse View Template</span></span>

<span data-ttu-id="4ed0b-558">W tym zadaniu zostanie dodany widok **przeglądania** w celu wyświetlenia albumów znalezionych dla określonego gatunku.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-558">In this task, you will add a **Browse** View to show the Albums found for a specific Genre.</span></span>

1. <span data-ttu-id="4ed0b-559">Przed utworzeniem nowego szablonu widoku należy skompilować projekt, aby wyświetlić okno dialogowe **Dodawanie widoku** o klasie **ViewModel** do użycia.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-559">Before creating the new View template, you should build the project so that the **Add View** Dialog knows about the **ViewModel** class to use.</span></span> <span data-ttu-id="4ed0b-560">Skompiluj projekt, wybierając element menu **kompilacja** , a następnie **Skompiluj MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-560">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>
2. <span data-ttu-id="4ed0b-561">Dodaj widok **przeglądania** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-561">Add a **Browse** View.</span></span> <span data-ttu-id="4ed0b-562">Aby to zrobić, kliknij prawym przyciskiem myszy metodę **przeglądania** akcji **StoreController** i kliknij polecenie **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-562">To do this, right-click in the **Browse** action method of the **StoreController** and click **Add View**.</span></span>
3. <span data-ttu-id="4ed0b-563">W oknie dialogowym **Dodawanie widoku** Sprawdź, czy nazwa widoku jest **przeszukiwana**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-563">In the **Add View** dialog box, verify that the View Name is **Browse**.</span></span> <span data-ttu-id="4ed0b-564">Zaznacz pole wyboru **Utwórz widok o jednoznacznie określonym typie** i wybierz pozycję **StoreBrowseViewModel** z listy rozwijanej **klasy modeli** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-564">Check the **Create a strongly-typed view** checkbox and select **StoreBrowseViewModel** from the **Model class** dropdown.</span></span> <span data-ttu-id="4ed0b-565">Pozostaw pozostałe pola wartością domyślną.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-565">Leave the other fields with their default value.</span></span> <span data-ttu-id="4ed0b-566">Następnie kliknij pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-566">Then click **Add**.</span></span>

    <span data-ttu-id="4ed0b-567">![Dodawanie widoku przeglądania](aspnet-mvc-4-fundamentals/_static/image29.png "Dodawanie widoku przeglądania")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-567">![Adding a Browse View](aspnet-mvc-4-fundamentals/_static/image29.png "Adding a Browse View")</span></span>

    <span data-ttu-id="4ed0b-568">*Dodawanie widoku przeglądania*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-568">*Adding a Browse View*</span></span>
4. <span data-ttu-id="4ed0b-569">Zmodyfikuj polecenie **Browse. cshtml** , aby wyświetlić informacje o gatunku, uzyskując dostęp do obiektu **StoreBrowseViewModel** , który jest przesyłany do szablonu widoku.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-569">Modify the **Browse.cshtml** to display the Genre's information, accessing the **StoreBrowseViewModel** object that is passed to the view template.</span></span> <span data-ttu-id="4ed0b-570">Aby to zrobić, Zastąp zawartość następującym: (C#)</span><span class="sxs-lookup"><span data-stu-id="4ed0b-570">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="4ed0b-571">Zadanie 5 — Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="4ed0b-571">Task 5 - Running the Application</span></span>

<span data-ttu-id="4ed0b-572">W tym zadaniu zostanie przetestowane, że metoda **Browse** pobiera albumy z akcji **Przeglądaj** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-572">In this task, you will test that the **Browse** method retrieves Albums from the **Browse** method action.</span></span>

1. <span data-ttu-id="4ed0b-573">Naciśnij klawisz **F5** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-573">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="4ed0b-574">Projekt zostanie uruchomiony na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-574">The project starts in the Home page.</span></span> <span data-ttu-id="4ed0b-575">Czy zmienić adres URL na **/Store/Browse? Gatunek = Disco** , aby sprawdzić, czy akcja zwraca dwa albumy.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-575">Change the URL to **/Store/Browse?Genre=Disco** to verify that the action returns two Albums.</span></span>

    <span data-ttu-id="4ed0b-576">![Przeglądanie albumów Disco w sklepie](aspnet-mvc-4-fundamentals/_static/image30.png "Przeglądanie albumów Disco w sklepie")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-576">![Browsing Store Disco Albums](aspnet-mvc-4-fundamentals/_static/image30.png "Browsing Store Disco Albums")</span></span>

    <span data-ttu-id="4ed0b-577">*Przeglądanie albumów Disco w sklepie*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-577">*Browsing Store Disco Albums*</span></span>

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a><span data-ttu-id="4ed0b-578">Zadanie 6 — Wyświetlanie informacji o określonym albumie</span><span class="sxs-lookup"><span data-stu-id="4ed0b-578">Task 6 - Displaying information About a Specific Album</span></span>

<span data-ttu-id="4ed0b-579">W tym zadaniu zostanie wdrożony widok **Magazyn/szczegóły** w celu wyświetlenia informacji o określonym albumie.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-579">In this task, you will implement the **Store/Details** view to display information about a specific album.</span></span> <span data-ttu-id="4ed0b-580">W tym ćwiczeniu w tym ćwiczeniu wszystko, co będzie wyświetlane na albumie, jest już zawarte w szablonie **widoku** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-580">In this Hands-on Lab, everything you will display about the album is already contained in the **View** template.</span></span> <span data-ttu-id="4ed0b-581">Dlatego zamiast tworzenia klasy **StoreDetailsViewModel** będzie używany bieżący szablon **StoreBrowseViewModel** , który przekazuje do niego album.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-581">So, instead of creating a **StoreDetailsViewModel** class, you will use the current **StoreBrowseViewModel** template passing the Album to it.</span></span>

1. <span data-ttu-id="4ed0b-582">W razie potrzeby Zamknij przeglądarkę, aby powrócić do okna programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-582">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="4ed0b-583">Dodaj nowy widok **szczegółów** dla metody akcji **Details** **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-583">Add a new **Details** view for the **StoreController**'s **Details** action method.</span></span> <span data-ttu-id="4ed0b-584">W tym celu kliknij prawym przyciskiem myszy metodę **Details** w klasie **StoreController** , a następnie kliknij polecenie **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-584">To do this, right-click the **Details** method in the **StoreController** class and click **Add View**.</span></span>
2. <span data-ttu-id="4ed0b-585">W oknie dialogowym **Dodawanie widoku** Sprawdź, czy **Nazwa widoku** to **szczegóły**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-585">In the **Add View** dialog, verify that the **View Name** is **Details**.</span></span> <span data-ttu-id="4ed0b-586">Zaznacz pole wyboru **Utwórz widok o jednoznacznie określonym typie** i wybierz opcję **album** z listy rozwijanej **Klasa modelu** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-586">Check the **Create a strongly-typed view** checkbox and select **Album** from the **Model class** drop-down.</span></span> <span data-ttu-id="4ed0b-587">Pozostaw pozostałe pola wartością domyślną.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-587">Leave the other fields with their default value.</span></span> <span data-ttu-id="4ed0b-588">Następnie kliknij pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-588">Then click **Add**.</span></span> <span data-ttu-id="4ed0b-589">Spowoduje to utworzenie i otwarcie pliku **\Views\Store\Details.cshtml** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-589">This will create and open a **\Views\Store\Details.cshtml** file.</span></span>

    <span data-ttu-id="4ed0b-590">![Dodawanie widoku szczegółów](aspnet-mvc-4-fundamentals/_static/image31.png "Dodawanie widoku szczegółów")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-590">![Adding a Details View](aspnet-mvc-4-fundamentals/_static/image31.png "Adding a Details View")</span></span>

    <span data-ttu-id="4ed0b-591">*Dodawanie widoku szczegółów*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-591">*Adding a Details View*</span></span>
3. <span data-ttu-id="4ed0b-592">Zmodyfikuj plik **details. cshtml** , aby wyświetlić informacje o albumie, uzyskując dostęp do obiektu **albumu** , który jest przesyłany do szablonu widoku.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-592">Modify the **Details.cshtml** file to display the Album's information, accessing the **Album** object that is passed to the view template.</span></span> <span data-ttu-id="4ed0b-593">Aby to zrobić, Zastąp zawartość następującym: (C#)</span><span class="sxs-lookup"><span data-stu-id="4ed0b-593">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="4ed0b-594">Zadanie 7 — Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="4ed0b-594">Task 7 - Running the Application</span></span>

<span data-ttu-id="4ed0b-595">W tym zadaniu zostanie przetestowany, że widok **szczegóły** pobiera informacje o albumie z metody **akcji szczegóły** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-595">In this task, you will test that the **Details** View retrieves Album's information from the **Details action** method.</span></span>

1. <span data-ttu-id="4ed0b-596">Naciśnij klawisz **F5** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-596">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="4ed0b-597">Projekt zostanie uruchomiony na stronie **głównej** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-597">The project starts in the **Home** page.</span></span> <span data-ttu-id="4ed0b-598">Zmień adres URL na **/Store/Details/5** , aby zweryfikować informacje o albumie.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-598">Change the URL to **/Store/Details/5** to verify the album's information.</span></span>

    <span data-ttu-id="4ed0b-599">![Szczegóły przeglądania albumów](aspnet-mvc-4-fundamentals/_static/image32.png "Szczegóły przeglądania albumów")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-599">![Browsing Albums Detail](aspnet-mvc-4-fundamentals/_static/image32.png "Browsing Albums Detail")</span></span>

    <span data-ttu-id="4ed0b-600">*Przeglądanie szczegółów albumu*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-600">*Browsing Album's Detail*</span></span>

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a><span data-ttu-id="4ed0b-601">Zadanie 8 — Dodawanie linków między stronami</span><span class="sxs-lookup"><span data-stu-id="4ed0b-601">Task 8 - Adding Links Between Pages</span></span>

<span data-ttu-id="4ed0b-602">W tym zadaniu dodasz w widoku sklepu link do odpowiedniego adresu URL **/Store/Browse** w polu Nazwa każdego gatunku.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-602">In this task, you will add a link in the Store View to have a link in every Genre name to the appropriate **/Store/Browse** URL.</span></span> <span data-ttu-id="4ed0b-603">W ten sposób po kliknięciu gatunku dla wystąpienia **Disco**zostanie przeszukany **/Store/Browse? gatunek = Disco** URL.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-603">This way, when you click on a Genre, for instance **Disco**, it will navigate to **/Store/Browse?genre=Disco** URL.</span></span>

1. <span data-ttu-id="4ed0b-604">W razie potrzeby Zamknij przeglądarkę, aby powrócić do okna programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-604">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="4ed0b-605">Zaktualizuj stronę **indeksu** , aby dodać łącze do strony **przeglądania** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-605">Update the **Index** page to add a link to the **Browse** page.</span></span> <span data-ttu-id="4ed0b-606">W tym celu w **Eksplorator rozwiązań** rozwiń folder **widoki** , a następnie folder **Store** i kliknij dwukrotnie stronę **index. cshtml** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-606">To do this, in the **Solution Explorer** expand the **Views** folder, then the **Store** folder and double-click the **Index.cshtml** page.</span></span>
2. <span data-ttu-id="4ed0b-607">Dodaj link do widoku przeglądania wskazującego wybrany gatunek.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-607">Add a link to the Browse view indicating the genre selected.</span></span> <span data-ttu-id="4ed0b-608">Aby to zrobić, Zastąp następujący wyróżniony kod w **&lt;li&gt;** tagów: (C#)</span><span class="sxs-lookup"><span data-stu-id="4ed0b-608">To do this, replace the following highlighted code within the **&lt;li&gt;** tags: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

   > [!NOTE]
   > <span data-ttu-id="4ed0b-609">innym podejściem będzie łączenie bezpośrednio ze stroną, z kodem podobnym do poniższego:</span><span class="sxs-lookup"><span data-stu-id="4ed0b-609">another approach would be linking directly to the page, with a code like the following:</span></span>
   > 
   > <span data-ttu-id="4ed0b-610">&lt;a href =&quot;/Store/Browse? gatunek =@genreName&quot;&gt;@genreName&lt;/a&gt;</span><span class="sxs-lookup"><span data-stu-id="4ed0b-610">&lt;a href=&quot;/Store/Browse?genre=@genreName&quot;&gt;@genreName&lt;/a&gt;</span></span>
   > 
   > <span data-ttu-id="4ed0b-611">Chociaż to podejście działa, zależy to od ciągu stałe.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-611">Although this approach works, it depends on a hardcoded string.</span></span> <span data-ttu-id="4ed0b-612">W przypadku późniejszej zmiany nazwy kontrolera należy ręcznie zmienić tę instrukcję.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-612">If you later rename the Controller, you will have to change this instruction manually.</span></span> <span data-ttu-id="4ed0b-613">Lepszym rozwiązaniem jest użycie metody **pomocnika HTML** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-613">A better alternative is to use an **HTML Helper** method.</span></span> <span data-ttu-id="4ed0b-614">ASP.NET MVC zawiera metodę pomocnika HTML, która jest dostępna dla takich zadań.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-614">ASP.NET MVC includes an HTML Helper method which is available for such tasks.</span></span> <span data-ttu-id="4ed0b-615">Metoda pomocnika **HTML. ActionLink ()** ułatwia tworzenie kodu HTML&lt;linków **&gt;** , co sprawia, że ścieżki URL są poprawnie zakodowane w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-615">The **Html.ActionLink()** helper method makes it easy to build HTML **&lt;a&gt;** links, making sure URL paths are properly URL encoded.</span></span>
   > 
   > <span data-ttu-id="4ed0b-616">Plik HTML. ActionLink ma kilka przeciążeń.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-616">Html.ActionLink has several overloads.</span></span> <span data-ttu-id="4ed0b-617">W tym ćwiczeniu będziesz używać jednego z trzech parametrów:</span><span class="sxs-lookup"><span data-stu-id="4ed0b-617">In this exercise you will use one that takes three parameters:</span></span>
   > 
   > 1. <span data-ttu-id="4ed0b-618">Tekst łącza, który będzie wyświetlał nazwę gatunku</span><span class="sxs-lookup"><span data-stu-id="4ed0b-618">Link text, which will display the Genre name</span></span>
   > 2. <span data-ttu-id="4ed0b-619">Nazwa akcji kontrolera (**Przeglądaj**)</span><span class="sxs-lookup"><span data-stu-id="4ed0b-619">Controller action name (**Browse**)</span></span>
   > 3. <span data-ttu-id="4ed0b-620">Wartości parametrów trasy, określające zarówno nazwę (**gatunek**), jak i wartość (**nazwę gatunku**)</span><span class="sxs-lookup"><span data-stu-id="4ed0b-620">Route parameter values, specifying both the name (**Genre**) and the value (**Genre name**)</span></span>

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a><span data-ttu-id="4ed0b-621">Zadanie 9 — Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="4ed0b-621">Task 9 - Running the Application</span></span>

<span data-ttu-id="4ed0b-622">W tym zadaniu sprawdzisz, że każdy gatunek jest wyświetlany z linkiem do odpowiedniego adresu URL **/Store/Browse** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-622">In this task, you will test that each Genre is displayed with a link to the appropriate **/Store/Browse** URL.</span></span>

1. <span data-ttu-id="4ed0b-623">Naciśnij klawisz **F5** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-623">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="4ed0b-624">Projekt zostanie uruchomiony na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-624">The project starts in the Home page.</span></span> <span data-ttu-id="4ed0b-625">Zmień adres URL na **/Store** , aby sprawdzić, czy każdy gatunek łączy się z odpowiednim adresem URL **/Store/Browse** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-625">Change the URL to **/Store** to verify that each Genre links to the appropriate **/Store/Browse** URL.</span></span>

    <span data-ttu-id="4ed0b-626">![Przeglądanie gatunków z linkami do strony przeglądania](aspnet-mvc-4-fundamentals/_static/image33.png "Przeglądanie gatunków z linkami do strony przeglądania")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-626">![Browsing Genres with links to Browse page](aspnet-mvc-4-fundamentals/_static/image33.png "Browsing Genres with links to Browse page")</span></span>

    <span data-ttu-id="4ed0b-627">*Przeglądanie gatunków z linkami do strony przeglądania*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-627">*Browsing Genres with links to Browse page*</span></span>

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a><span data-ttu-id="4ed0b-628">Zadanie 10 — Używanie dynamicznej kolekcji ViewModel do przekazywania wartości</span><span class="sxs-lookup"><span data-stu-id="4ed0b-628">Task 10 - Using Dynamic ViewModel Collection to Pass Values</span></span>

<span data-ttu-id="4ed0b-629">W tym zadaniu przedstawiono prostą i zaawansowaną metodę przekazywania wartości między kontrolerem a widokiem bez wprowadzania żadnych zmian w modelu.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-629">In this task, you will learn a simple and powerful method to pass values between the Controller and the View without making any changes in the Model.</span></span> <span data-ttu-id="4ed0b-630">ASP.NET MVC 4 udostępnia kolekcje &quot;ViewModel&quot;, które można przypisać do dowolnej wartości dynamicznej i uzyskać do nich dostęp tylko w kontrolerach i widokach.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-630">ASP.NET MVC 4 provides the collection &quot;ViewModel&quot;, which can be assigned to any dynamic value and accessed inside controllers and views as well.</span></span>

<span data-ttu-id="4ed0b-631">Teraz użyjesz dynamicznej kolekcji ViewBag, aby przekazać listę &quot;ych&quot; **gatunku** z kontrolera do widoku.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-631">You will now use the ViewBag dynamic collection to pass a list of &quot;**Starred genres**&quot; from the controller to the view.</span></span> <span data-ttu-id="4ed0b-632">Widok indeksu magazynu będzie miał dostęp do **ViewModel** i wyświetlania informacji.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-632">The Store Index view will access to **ViewModel** and display the information.</span></span>

1. <span data-ttu-id="4ed0b-633">W razie potrzeby Zamknij przeglądarkę, aby powrócić do okna programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-633">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="4ed0b-634">Otwórz **StoreController.cs** i zmodyfikuj metodę **index** , aby utworzyć listę gatunków oznaczona gwiazdkami w kolekcji ViewModel:</span><span class="sxs-lookup"><span data-stu-id="4ed0b-634">Open **StoreController.cs** and modify **Index** method to create a list of starred genres into ViewModel collection :</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > <span data-ttu-id="4ed0b-635">Aby uzyskać dostęp do właściwości, można również użyć składni **ViewBag [&quot;oznaczona gwiazdkami&quot;]** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-635">You could also use the syntax **ViewBag[&quot;Starred&quot;]** to access the properties.</span></span>
2. <span data-ttu-id="4ed0b-636">Ikona gwiazdki **&quot;oznaczona gwiazdkami. png&quot;** jest zawarta w folderze **Source\Assets\Images** w tym laboratorium.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-636">The star icon **&quot;starred.png&quot;** is included in the **Source\Assets\Images** folder of this lab.</span></span> <span data-ttu-id="4ed0b-637">Aby dodać ją do aplikacji, przeciągnij jej zawartość z okna **Eksploratora Windows** do **Eksplorator rozwiązań** w programie Visual Web Developer Express, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="4ed0b-637">In order to add it to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Web Developer Express, as shown below:</span></span>

    <span data-ttu-id="4ed0b-638">![Dodawanie obrazu gwiazdy do rozwiązania](aspnet-mvc-4-fundamentals/_static/image34.png "Dodawanie obrazu gwiazdy do rozwiązania")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-638">![Adding star image to the solution](aspnet-mvc-4-fundamentals/_static/image34.png "Adding star image to the solution")</span></span>

    <span data-ttu-id="4ed0b-639">*Dodawanie obrazu gwiazdy do rozwiązania*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-639">*Adding star image to the solution*</span></span>
3. <span data-ttu-id="4ed0b-640">Otwórz widok **Store/index. cshtml** i zmodyfikuj zawartość.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-640">Open the view **Store/Index.cshtml** and modify the content.</span></span> <span data-ttu-id="4ed0b-641">Właściwość &quot;oznaczona gwiazdkami&quot; zostanie odczytana w kolekcji **ViewBag** i zażądać, jeśli nazwa bieżącego gatunku znajduje się na liście.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-641">You will read the &quot;starred&quot; property in the **ViewBag** collection, and ask if the current genre name is in the list.</span></span> <span data-ttu-id="4ed0b-642">W takim przypadku zostanie wyświetlona ikona gwiazdki z prawej strony linku do gatunku.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-642">In that case you will show a star icon right to the genre link.</span></span>
   <span data-ttu-id="4ed0b-643">(C#)</span><span class="sxs-lookup"><span data-stu-id="4ed0b-643">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a><span data-ttu-id="4ed0b-644">Zadanie 11 — uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="4ed0b-644">Task 11 - Running the Application</span></span>

<span data-ttu-id="4ed0b-645">W tym zadaniu sprawdzisz, że w gatunku oznaczona gwiazdkami zostanie wyświetlona ikona gwiazdki.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-645">In this task, you will test that the starred genres display a star icon.</span></span>

1. <span data-ttu-id="4ed0b-646">Naciśnij klawisz **F5** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-646">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="4ed0b-647">Projekt zostanie uruchomiony na stronie **głównej** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-647">The project starts in the **Home** page.</span></span> <span data-ttu-id="4ed0b-648">Zmień adres URL na **/Store** , aby upewnić się, że każdy proponowany gatunek ma etykietę respektują:</span><span class="sxs-lookup"><span data-stu-id="4ed0b-648">Change the URL to **/Store** to verify that each featured genre has the respecting label:</span></span>

    <span data-ttu-id="4ed0b-649">![Przeglądanie gatunków przy użyciu elementów oznaczona gwiazdkami](aspnet-mvc-4-fundamentals/_static/image35.png "Przeglądanie gatunków przy użyciu elementów oznaczona gwiazdkami")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-649">![Browsing Genres with starred elements](aspnet-mvc-4-fundamentals/_static/image35.png "Browsing Genres with starred elements")</span></span>

    <span data-ttu-id="4ed0b-650">*Przeglądanie gatunków przy użyciu elementów oznaczona gwiazdkami*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-650">*Browsing Genres with starred elements*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a><span data-ttu-id="4ed0b-651">Ćwiczenie 7: kolano wokół ASP.NET MVC 4 nowy szablon</span><span class="sxs-lookup"><span data-stu-id="4ed0b-651">Exercise 7: A lap around ASP.NET MVC 4 new template</span></span>

<span data-ttu-id="4ed0b-652">W tym ćwiczeniu zapoznajesz ulepszenia w szablonach projektów ASP.NET MVC 4, biorąc pod uwagę najbardziej odpowiednie funkcje nowego szablonu.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-652">In this exercise, you will explore the enhancements in the ASP.NET MVC 4 project templates, taking a look at the most relevant features of the new template.</span></span>

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a><span data-ttu-id="4ed0b-653">Zadanie 1. Eksplorowanie szablonu aplikacji internetowej ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="4ed0b-653">Task 1: Exploring the ASP.NET MVC 4 Internet Application Template</span></span>

1. <span data-ttu-id="4ed0b-654">Jeśli nie jest jeszcze otwarty, uruchom **vs Express for Web**</span><span class="sxs-lookup"><span data-stu-id="4ed0b-654">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="4ed0b-655">Wybierz **plik | Nowy |** Polecenie menu projektu.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-655">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="4ed0b-656">W oknie dialogowym **Nowy projekt** wybierz **wizualizację C#| Szablon sieci Web** w okienku po lewej stronie, a następnie wybierz **aplikację sieci Web ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-656">In the **New Project** dialog, select the **Visual C#|Web** template on the left pane tree, and choose the **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="4ed0b-657">**Nazwij** projekt *MusicStore* i zaktualizuj **nazwę rozwiązania** , aby *rozpocząć*, a następnie wybierz lokalizację (lub pozostaw wartość domyślną), a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-657">**Name** the project *MusicStore* and update the **solution name** to *Begin*, then select a location (or leave the default) and click **OK**.</span></span>

    <span data-ttu-id="4ed0b-658">![Tworzenie nowego projektu ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image36.png "Tworzenie nowego projektu ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-658">![Creating a new ASP.NET MVC 4 Project](aspnet-mvc-4-fundamentals/_static/image36.png "Creating a new ASP.NET MVC 4 Project")</span></span>

    <span data-ttu-id="4ed0b-659">*Tworzenie nowego projektu ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-659">*Creating a new ASP.NET MVC 4 Project*</span></span>
3. <span data-ttu-id="4ed0b-660">W oknie dialogowym **Nowy projekt ASP.NET MVC 4** wybierz szablon projektu **aplikacji internetowej** i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-660">In the **New ASP.NET MVC 4 Project** dialog, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="4ed0b-661">Zwróć uwagę, że można wybrać opcję Razor lub ASPX jako aparat widoku.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-661">Notice you can select either Razor or ASPX as the view engine.</span></span>

    <span data-ttu-id="4ed0b-662">![Tworzenie nowej aplikacji internetowej ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image37.png "Tworzenie nowej aplikacji internetowej ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-662">![Creating a new ASP.NET MVC 4 Internet Application](aspnet-mvc-4-fundamentals/_static/image37.png "Creating a new ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="4ed0b-663">*Tworzenie nowej aplikacji internetowej ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-663">*Creating a new ASP.NET MVC 4 Internet Application*</span></span>

    > [!NOTE]
    > <span data-ttu-id="4ed0b-664">Składnia Razor wprowadzono w ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-664">Razor syntax has been introduced in ASP.NET MVC 3.</span></span> <span data-ttu-id="4ed0b-665">Celem jest Minimalizacja liczby znaków i naciśnięć klawiszy wymaganych w pliku, co pozwala na szybkie i płynne przepływność kodowania.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-665">Its goal is to minimize the number of characters and keystrokes required in a file, enabling a fast and fluid coding workflow.</span></span> <span data-ttu-id="4ed0b-666">Razor wykorzystuje istniejące C#umiejętności języka/vb (lub inne) i dostarcza składnię znaczników szablonów, która umożliwia tworzenie doskonałych przepływów pracy konstrukcyjnych html.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-666">Razor leverages existing C#/VB (or other) language skills and delivers a template markup syntax that enables an awesome HTML construction workflow.</span></span>
4. <span data-ttu-id="4ed0b-667">Naciśnij klawisz **F5** , aby uruchomić rozwiązanie i zapoznać się z odnowionym szablonem.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-667">Press **F5** to run the solution and see the renewed template.</span></span> <span data-ttu-id="4ed0b-668">Można wyewidencjonować następujące funkcje:</span><span class="sxs-lookup"><span data-stu-id="4ed0b-668">You can check out the following features:</span></span>

    1. <span data-ttu-id="4ed0b-669">**Szablony w stylu Modern**</span><span class="sxs-lookup"><span data-stu-id="4ed0b-669">**Modern-style templates**</span></span>

        <span data-ttu-id="4ed0b-670">Szablony zostały odnowione, co zapewnia bardziej nowoczesny wygląd stylów.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-670">The templates have been renewed, providing more modern-looking styles.</span></span>

        <span data-ttu-id="4ed0b-671">![Szablony z ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image38.png "Szablony z ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-671">![ASP.NET MVC 4 restyled templates](aspnet-mvc-4-fundamentals/_static/image38.png "ASP.NET MVC 4 restyled templates")</span></span>

        <span data-ttu-id="4ed0b-672">*Szablony z ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-672">*ASP.NET MVC 4 restyled templates*</span></span>
    2. <span data-ttu-id="4ed0b-673">**Renderowanie adaptacyjne**</span><span class="sxs-lookup"><span data-stu-id="4ed0b-673">**Adaptive Rendering**</span></span>

        <span data-ttu-id="4ed0b-674">Sprawdź zmianę rozmiaru okna przeglądarki i zwróć uwagę na to, jak układ strony dynamicznie dostosowuje się do nowego rozmiaru okna.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-674">Check out resizing the browser window and notice how the page layout dynamically adapts to the new window size.</span></span> <span data-ttu-id="4ed0b-675">Te szablony wykorzystują technikę renderowania adaptacyjnego do prawidłowego renderowania na platformach stacjonarnych i mobilnych bez żadnego dostosowania.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-675">These templates use the adaptive rendering technique to render properly in both desktop and mobile platforms without any customization.</span></span>

        <span data-ttu-id="4ed0b-676">![Szablon projektu ASP.NET MVC 4 w różnych rozmiarach przeglądarki](aspnet-mvc-4-fundamentals/_static/image39.png "Szablon projektu ASP.NET MVC 4 w różnych rozmiarach przeglądarki")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-676">![ASP.NET MVC 4 project template in different browser sizes](aspnet-mvc-4-fundamentals/_static/image39.png "ASP.NET MVC 4 project template in different browser sizes")</span></span>

        <span data-ttu-id="4ed0b-677">*Szablon projektu ASP.NET MVC 4 w różnych rozmiarach przeglądarki*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-677">*ASP.NET MVC 4 project template in different browser sizes*</span></span>
5. <span data-ttu-id="4ed0b-678">Zamknij przeglądarkę, aby zatrzymać debuger i wrócić do programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-678">Close the browser to stop the debugger and return to Visual Studio.</span></span>
6. <span data-ttu-id="4ed0b-679">Teraz możesz eksplorować rozwiązanie i zapoznać się z kilkoma nowymi funkcjami wprowadzonymi przez ASP.NET MVC 4 w szablonie projektu.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-679">Now you are able to explore the solution and check out some of the new features introduced by ASP.NET MVC 4 in the project template.</span></span>

    <span data-ttu-id="4ed0b-680">![ASP.NET MVC4 — Internet-aplikacja-projekt-szablon](aspnet-mvc-4-fundamentals/_static/image40.png "Szablon projektu aplikacji internetowej ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-680">![ASP.NET MVC4-internet-application-project-template](aspnet-mvc-4-fundamentals/_static/image40.png "The ASP.NET MVC 4 Internet Application Project Template")</span></span>

    <span data-ttu-id="4ed0b-681">*Szablon projektu aplikacji internetowej ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-681">*The ASP.NET MVC 4 Internet Application Project Template*</span></span>

   1. <span data-ttu-id="4ed0b-682">**Znaczniki HTML5**</span><span class="sxs-lookup"><span data-stu-id="4ed0b-682">**HTML5 markup**</span></span>

       <span data-ttu-id="4ed0b-683">Przeglądaj widoki szablonów, aby dowiedzieć się więcej o nowym znaczniku motywu, np. Otwórz widok **about. cshtml** w folderze **głównym** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-683">Browse template views to find out the new theme markup, for example open **About.cshtml** view within **Home** folder.</span></span>

       <span data-ttu-id="4ed0b-684">![Nowy szablon, używanie języka Razor i znaczników HTML5](aspnet-mvc-4-fundamentals/_static/image41.png "Nowy szablon, używanie języka Razor i znaczników HTML5")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-684">![New template, using Razor and HTML5 markup](aspnet-mvc-4-fundamentals/_static/image41.png "New template, using Razor and HTML5 markup")</span></span>

       <span data-ttu-id="4ed0b-685">*Nowy szablon, używanie języka Razor i znaczników HTML5*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-685">*New template, using Razor and HTML5 markup*</span></span>
   2. <span data-ttu-id="4ed0b-686">**Uwzględnione biblioteki JavaScript**</span><span class="sxs-lookup"><span data-stu-id="4ed0b-686">**JavaScript libraries included**</span></span>

      1. <span data-ttu-id="4ed0b-687">**jQuery**: jQuery upraszcza przechodzenie do dokumentów HTML, obsługę zdarzeń, animowanie i interakcje AJAX.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-687">**jQuery**: jQuery simplifies HTML document traversing, event handling, animating, and Ajax interactions.</span></span>
      2. <span data-ttu-id="4ed0b-688">**interfejs użytkownika jQuery**: Ta biblioteka zawiera abstrakcje dla interakcji i animacji niskiego poziomu, zaawansowanych efektów i elementów widget z motywem, utworzonych na podstawie biblioteki jQuery JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-688">**jQuery UI**: This library provides abstractions for low-level interaction and animation, advanced effects and themeable widgets, built on top of the jQuery JavaScript Library.</span></span>

         > [!NOTE]
         > <span data-ttu-id="4ed0b-689">Informacje na temat interfejsu użytkownika jQuery i jQuery można znaleźć w [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="4ed0b-689">You can learn about jQuery and jQuery UI in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span></span>
      3. <span data-ttu-id="4ed0b-690">**KnockoutJS**: szablon domyślny ASP.NET MVC 4 zawiera teraz **KNOCKOUTJS**, środowisko MVVM języka JavaScript, które umożliwia tworzenie rozbudowanych i wysoce reagujących aplikacji sieci Web przy użyciu języków JavaScript i HTML.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-690">**KnockoutJS**: The ASP.NET MVC 4 default template now includes **KnockoutJS**, a JavaScript MVVM framework that lets you create rich and highly responsive web applications using JavaScript and HTML.</span></span> <span data-ttu-id="4ed0b-691">Podobnie jak w przypadku bibliotek interfejsu użytkownika ASP.NET MVC 3, jQuery i jQuery są również dołączone do ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-691">Like in ASP.NET MVC 3, jQuery and jQuery UI libraries are also included in ASP.NET MVC 4.</span></span>

          > [!NOTE]
          > <span data-ttu-id="4ed0b-692">Więcej informacji na temat biblioteki KnockOutJS można znaleźć w tym łączu: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).</span><span class="sxs-lookup"><span data-stu-id="4ed0b-692">You can get more information about KnockOutJS library in this link: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).</span></span>
      4. <span data-ttu-id="4ed0b-693">**Modernizacja**: Ta biblioteka działa automatycznie, dzięki czemu witryna jest zgodna ze starszymi przeglądarkami w przypadku korzystania z technologii HTML5 i CSS3.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-693">**Modernizr**: This library runs automatically, making your site compatible with older browsers when using HTML5 and CSS3 technologies.</span></span>

          > [!NOTE]
          > <span data-ttu-id="4ed0b-694">W tym łączu można uzyskać więcej informacji na temat biblioteki programu modernizacji: [http://www.modernizr.com/](http://www.modernizr.com/).</span><span class="sxs-lookup"><span data-stu-id="4ed0b-694">You can get more information about Modernizr library in this link: [http://www.modernizr.com/](http://www.modernizr.com/).</span></span>
   3. <span data-ttu-id="4ed0b-695">**SimpleMembership dołączone do rozwiązania**</span><span class="sxs-lookup"><span data-stu-id="4ed0b-695">**SimpleMembership included in the solution**</span></span>

       <span data-ttu-id="4ed0b-696">SimpleMembership został zaprojektowany jako zamiennik dla poprzedniej roli ASP.NET i systemu dostawcy członkostwa.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-696">SimpleMembership has been designed as a replacement for the previous ASP.NET Role and Membership provider system.</span></span> <span data-ttu-id="4ed0b-697">Ma wiele nowych funkcji, które ułatwiają deweloperom Zabezpieczanie stron sieci Web w bardziej elastyczny sposób.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-697">It has many new features that make it easier for the developer to secure web pages in a more flexible way.</span></span>

       <span data-ttu-id="4ed0b-698">Szablon internetowy już skonfigurował kilka rzeczy do integracji SimpleMembership, na przykład elementu AccountController jest przygotowana do korzystania z OAuthWebSecurity (w przypadku rejestracji, logowania, zarządzania itp.) i zabezpieczeń sieci Web.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-698">The Internet template already has set up a few things to integrate SimpleMembership, for example, the AccountController is prepared to use OAuthWebSecurity (for OAuth account registration, login, management, etc.) and Web Security.</span></span>

       <span data-ttu-id="4ed0b-699">![SimpleMembership dołączone do rozwiązania](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership dołączone do rozwiązania")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-699">![SimpleMembership Included in the solution](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership Included in the solution")</span></span>

       <span data-ttu-id="4ed0b-700">*SimpleMembership dołączone do rozwiązania*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-700">*SimpleMembership Included in the solution*</span></span>

       > [!NOTE]
       > <span data-ttu-id="4ed0b-701">Więcej informacji na temat [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) w witrynie MSDN.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-701">Find more information about [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) in MSDN.</span></span>

> [!NOTE]
> <span data-ttu-id="4ed0b-702">Ponadto tę aplikację można wdrożyć w witrynach sieci Web systemu Windows Azure, które zostały opisane w [dodatku B: publikowanie aplikacji ASP.NET MVC 4 przy użyciu Web Deploy](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="4ed0b-702">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="4ed0b-703">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="4ed0b-703">Summary</span></span>

<span data-ttu-id="4ed0b-704">Wypełniając to praktyczne laboratorium, uczysz się podstaw ASP.NET MVC:</span><span class="sxs-lookup"><span data-stu-id="4ed0b-704">By completing this Hands-On Lab you have learned the fundamentals of ASP.NET MVC:</span></span>

- <span data-ttu-id="4ed0b-705">Podstawowe elementy aplikacji MVC oraz sposób ich działania</span><span class="sxs-lookup"><span data-stu-id="4ed0b-705">The core elements of an MVC application and how they interact</span></span>
- <span data-ttu-id="4ed0b-706">Jak utworzyć aplikację ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="4ed0b-706">How to create an ASP.NET MVC Application</span></span>
- <span data-ttu-id="4ed0b-707">Jak dodać i skonfigurować kontrolery do obsługi parametrów przesyłanych za pomocą adresu URL i ciągu QueryString</span><span class="sxs-lookup"><span data-stu-id="4ed0b-707">How to add and configure Controllers to handle parameters passed through the URL and querystring</span></span>
- <span data-ttu-id="4ed0b-708">Jak dodać stronę wzorcową układu, aby skonfigurować szablon dla typowej zawartości HTML, arkusz stylów, aby wzbogacić wygląd i działanie oraz szablon widoku w celu wyświetlenia zawartości HTML</span><span class="sxs-lookup"><span data-stu-id="4ed0b-708">How to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel and a View template to display HTML content</span></span>
- <span data-ttu-id="4ed0b-709">Jak użyć wzorca ViewModel do przekazywania właściwości do szablonu widoku, aby wyświetlić informacje dynamiczne</span><span class="sxs-lookup"><span data-stu-id="4ed0b-709">How to use the ViewModel pattern for passing properties to the View template to display dynamic information</span></span>
- <span data-ttu-id="4ed0b-710">Jak używać parametrów przesyłanych do kontrolerów w szablonie widoku</span><span class="sxs-lookup"><span data-stu-id="4ed0b-710">How to use parameters passed to Controllers in the View template</span></span>
- <span data-ttu-id="4ed0b-711">Jak dodać linki do stron wewnątrz aplikacji ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="4ed0b-711">How to add links to pages inside the ASP.NET MVC application</span></span>
- <span data-ttu-id="4ed0b-712">Jak dodać właściwości dynamiczne i używać ich w widoku</span><span class="sxs-lookup"><span data-stu-id="4ed0b-712">How to add and use dynamic properties in a View</span></span>
- <span data-ttu-id="4ed0b-713">Ulepszenia w szablonach projektów ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="4ed0b-713">The enhancements in the ASP.NET MVC 4 project templates</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="4ed0b-714">Dodatek A: Instalowanie Visual Studio Express 2012 dla sieci Web</span><span class="sxs-lookup"><span data-stu-id="4ed0b-714">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="4ed0b-715">Można zainstalować **Microsoft Visual Studio Express 2012 dla sieci Web** lub innej &quot;Express&quot; wersji za pomocą **[Instalator platformy Microsoft Web](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-715">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="4ed0b-716">Poniższe instrukcje przeprowadzą Cię przez kroki wymagane do zainstalowania *programu Visual Studio Express 2012 for Web* przy użyciu *Instalator platformy Microsoft Web*.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-716">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="4ed0b-717">Przejdź do [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="4ed0b-717">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="4ed0b-718">Alternatywnie, jeśli zainstalowano już Instalatora platformy sieci Web, można go otworzyć i wyszukać produkt &quot;<em>Visual Studio Express 2012 for Web przy użyciu zestawu Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-718">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="4ed0b-719">Kliknij pozycję **Zainstaluj teraz**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-719">Click on **Install Now**.</span></span> <span data-ttu-id="4ed0b-720">Jeśli nie masz **Instalatora platformy sieci Web** , nastąpi przekierowanie do pobrania i zainstalowania go w pierwszej kolejności.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-720">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="4ed0b-721">Po otwarciu **Instalatora platformy sieci Web** kliknij przycisk **Zainstaluj** , aby rozpocząć instalację.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-721">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="4ed0b-722">![Zainstaluj Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "Zainstaluj Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-722">![Install Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="4ed0b-723">*Zainstaluj Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-723">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="4ed0b-724">Przeczytaj wszystkie licencje i postanowienia produktów, a następnie kliknij przycisk **Akceptuję** , aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-724">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akceptowanie postanowień licencyjnych](aspnet-mvc-4-fundamentals/_static/image44.png)

    <span data-ttu-id="4ed0b-726">*Akceptowanie postanowień licencyjnych*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-726">*Accepting the license terms*</span></span>
5. <span data-ttu-id="4ed0b-727">Poczekaj na zakończenie procesu pobierania i instalacji.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-727">Wait until the downloading and installation process completes.</span></span>

    ![Postęp instalacji](aspnet-mvc-4-fundamentals/_static/image45.png)

    <span data-ttu-id="4ed0b-729">*Postęp instalacji*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-729">*Installation progress*</span></span>
6. <span data-ttu-id="4ed0b-730">Po zakończeniu instalacji kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-730">When the installation completes, click **Finish**.</span></span>

    ![Instalacja zakończona](aspnet-mvc-4-fundamentals/_static/image46.png)

    <span data-ttu-id="4ed0b-732">*Instalacja zakończona*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-732">*Installation completed*</span></span>
7. <span data-ttu-id="4ed0b-733">Kliknij przycisk **Zakończ** , aby zamknąć Instalatora platformy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-733">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="4ed0b-734">Aby otworzyć Visual Studio Express dla sieci Web, przejdź do ekranu **startowego** i zacznij pisać &quot;**vs Express**&quot;, a następnie kliknij kafelek **vs Express for Web** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-734">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Kafelek VS Express for Web](aspnet-mvc-4-fundamentals/_static/image47.png)

    <span data-ttu-id="4ed0b-736">*Kafelek VS Express for Web*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-736">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="4ed0b-737">Dodatek B: publikowanie aplikacji ASP.NET MVC 4 przy użyciu Web Deploy</span><span class="sxs-lookup"><span data-stu-id="4ed0b-737">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="4ed0b-738">W tym dodatku pokazano, jak utworzyć nową witrynę sieci Web na podstawie portal zarządzania systemu Windows Azure i opublikować aplikację uzyskaną w wyniku działania laboratorium, korzystając z funkcji publikowania Web Deploy dostępnej w systemie Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-738">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="4ed0b-739">Zadanie 1 — Tworzenie nowej witryny sieci Web z poziomu portalu systemu Windows Azure</span><span class="sxs-lookup"><span data-stu-id="4ed0b-739">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="4ed0b-740">Przejdź do [Portal zarządzania systemu Windows Azure](https://manage.windowsazure.com/) i zaloguj się przy użyciu poświadczeń firmy Microsoft skojarzonych z Twoją subskrypcją.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-740">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="4ed0b-741">System Windows Azure umożliwia bezpłatne hostowanie witryn sieci Web 10 ASP.NET, a następnie skalowanie ich w miarę wzrostu ruchu.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-741">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="4ed0b-742">Możesz utworzyć konto w [tym miejscu](https://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="4ed0b-742">You can sign up [here](https://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="4ed0b-743">![Zaloguj się do systemu Windows Azure Portal](aspnet-mvc-4-fundamentals/_static/image48.png "Zaloguj się do systemu Windows Azure Portal")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-743">![Log on to Windows Azure portal](aspnet-mvc-4-fundamentals/_static/image48.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="4ed0b-744">*Zaloguj się do usługi Windows Azure portal zarządzania*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-744">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="4ed0b-745">Na pasku poleceń kliknij pozycję **Nowy** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-745">Click **New** on the command bar.</span></span>

    <span data-ttu-id="4ed0b-746">![Tworzenie nowej witryny sieci Web](aspnet-mvc-4-fundamentals/_static/image49.png "Tworzenie nowej witryny sieci Web")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-746">![Creating a new Web Site](aspnet-mvc-4-fundamentals/_static/image49.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="4ed0b-747">*Tworzenie nowej witryny sieci Web*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-747">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="4ed0b-748">Kliknij pozycję **obliczeniowe** | **witrynie sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-748">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="4ed0b-749">Następnie wybierz opcję **szybkie tworzenie** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-749">Then select **Quick Create** option.</span></span> <span data-ttu-id="4ed0b-750">Podaj dostępny adres URL dla nowej witryny sieci Web i kliknij pozycję **Utwórz witrynę sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-750">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="4ed0b-751">Witryna sieci Web systemu Windows Azure jest hostem aplikacji sieci Web działającej w chmurze, którą można kontrolować i zarządzać nią.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-751">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="4ed0b-752">Opcja szybkie tworzenie umożliwia wdrożenie ukończonej aplikacji sieci Web w witrynie sieci Web systemu Windows Azure spoza portalu.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-752">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="4ed0b-753">Nie obejmuje to kroków związanych z konfigurowaniem bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-753">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="4ed0b-754">![Tworzenie nowej witryny sieci Web przy użyciu funkcji szybkiego tworzenia](aspnet-mvc-4-fundamentals/_static/image50.png "Tworzenie nowej witryny sieci Web przy użyciu funkcji szybkiego tworzenia")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-754">![Creating a new Web Site using Quick Create](aspnet-mvc-4-fundamentals/_static/image50.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="4ed0b-755">*Tworzenie nowej witryny sieci Web przy użyciu funkcji szybkiego tworzenia*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-755">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="4ed0b-756">Poczekaj na utworzenie nowej **witryny sieci Web** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-756">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="4ed0b-757">Po utworzeniu witryny sieci Web kliknij link pod kolumną **adres URL** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-757">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="4ed0b-758">Sprawdź, czy nowa witryna sieci Web działa.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-758">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="4ed0b-759">![Przeglądanie w nowej witrynie sieci Web](aspnet-mvc-4-fundamentals/_static/image51.png "Przeglądanie w nowej witrynie sieci Web")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-759">![Browsing to the new web site](aspnet-mvc-4-fundamentals/_static/image51.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="4ed0b-760">*Przeglądanie w nowej witrynie sieci Web*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-760">*Browsing to the new web site*</span></span>

    <span data-ttu-id="4ed0b-761">![Uruchomiona witryna sieci Web](aspnet-mvc-4-fundamentals/_static/image52.png "Uruchomiona witryna sieci Web")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-761">![Web site running](aspnet-mvc-4-fundamentals/_static/image52.png "Web site running")</span></span>

    <span data-ttu-id="4ed0b-762">*Uruchomiona witryna sieci Web*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-762">*Web site running*</span></span>
6. <span data-ttu-id="4ed0b-763">Wróć do portalu i kliknij nazwę witryny sieci Web pod kolumną **Nazwa** , aby wyświetlić strony zarządzania.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-763">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="4ed0b-764">![Otwieranie stron zarządzania witryną sieci Web](aspnet-mvc-4-fundamentals/_static/image53.png "Otwieranie stron zarządzania witryną sieci Web")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-764">![Opening the web site management pages](aspnet-mvc-4-fundamentals/_static/image53.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="4ed0b-765">*Otwieranie stron zarządzania witryną sieci Web*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-765">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="4ed0b-766">Na stronie **pulpit nawigacyjny** w sekcji **szybki przegląd** kliknij link **Pobierz profil publikowania** .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-766">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="4ed0b-767">*Profil publikowania* zawiera wszystkie informacje wymagane do opublikowania aplikacji sieci Web w witrynie sieci Web systemu Windows Azure dla każdej z włączonych metod publikacji.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-767">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="4ed0b-768">Profil publikowania zawiera adresy URL, poświadczenia użytkownika i ciągi bazy danych wymagane do nawiązania połączenia i uwierzytelniania dla każdego punktu końcowego, dla którego jest włączona metoda publikacji.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-768">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="4ed0b-769">**Program Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** i **Microsoft Visual Studio 2012** obsługują odczytywanie profilów publikowania w celu zautomatyzowania konfiguracji tych programów do publikowania aplikacji sieci Web w usłudze Microsoft Azure Websites.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-769">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="4ed0b-770">![Pobieranie profilu publikowania witryny sieci Web](aspnet-mvc-4-fundamentals/_static/image54.png "Pobieranie profilu publikowania witryny sieci Web")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-770">![Downloading the web site publish profile](aspnet-mvc-4-fundamentals/_static/image54.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="4ed0b-771">*Pobieranie profilu publikowania witryny sieci Web*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-771">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="4ed0b-772">Pobierz plik profilu publikowania do znanej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-772">Download the publish profile file to a known location.</span></span> <span data-ttu-id="4ed0b-773">W tym ćwiczeniu zobaczysz, jak używać tego pliku do publikowania aplikacji sieci Web w witrynach sieci Web systemu Windows Azure z programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-773">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="4ed0b-774">![Zapisywanie pliku profilu publikowania](aspnet-mvc-4-fundamentals/_static/image55.png "Zapisywanie profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-774">![Saving the publish profile file](aspnet-mvc-4-fundamentals/_static/image55.png "Saving the publish profile")</span></span>

    <span data-ttu-id="4ed0b-775">*Zapisywanie pliku profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-775">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="4ed0b-776">Zadanie 2 — Konfigurowanie serwera bazy danych</span><span class="sxs-lookup"><span data-stu-id="4ed0b-776">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="4ed0b-777">Jeśli aplikacja korzysta z baz danych SQL Server, należy utworzyć SQL Database serwer.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-777">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="4ed0b-778">Jeśli chcesz wdrożyć prostą aplikację, która nie używa SQL Server można pominąć to zadanie.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-778">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="4ed0b-779">Do przechowywania bazy danych aplikacji potrzebny jest serwer SQL Database.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-779">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="4ed0b-780">Serwery SQL Database z subskrypcji można wyświetlić w portalu zarządzania systemu Windows Azure w obszarze **bazy danych SQL** | **serwery** | **pulpitu nawigacyjnego serwera**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-780">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="4ed0b-781">Jeśli nie masz utworzonego serwera, możesz go utworzyć przy użyciu przycisku **Dodaj** na pasku poleceń.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-781">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="4ed0b-782">Zanotuj **nazwę serwera i adres URL, nazwę logowania administratora i hasło**, ponieważ zostaną one użyte w następnych zadaniach.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-782">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="4ed0b-783">Nie Twórz jeszcze bazy danych, ponieważ zostanie ona utworzona na późniejszym etapie.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-783">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="4ed0b-784">![Pulpit nawigacyjny serwera SQL Database](aspnet-mvc-4-fundamentals/_static/image56.png "Pulpit nawigacyjny serwera SQL Database")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-784">![SQL Database Server Dashboard](aspnet-mvc-4-fundamentals/_static/image56.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="4ed0b-785">*Pulpit nawigacyjny serwera SQL Database*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-785">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="4ed0b-786">W następnym zadaniu zostanie przetestowane połączenie z bazą danych z programu Visual Studio. z tego powodu należy uwzględnić lokalny adres IP na liście **dozwolonych adresów IP**serwera.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-786">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="4ed0b-787">W tym celu kliknij pozycję **Konfiguruj**, wybierz adres IP z **bieżącego adresu IP klienta** i wklej go w polach tekstowych **początkowy adres IP** i **końcowy adres IP** , a następnie kliknij przycisk ![Dodaj-Client-IP-Address-OK-Button](aspnet-mvc-4-fundamentals/_static/image57.png).</span><span class="sxs-lookup"><span data-stu-id="4ed0b-787">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) button.</span></span>

    ![Dodawanie adresu IP klienta](aspnet-mvc-4-fundamentals/_static/image58.png)

    <span data-ttu-id="4ed0b-789">*Dodawanie adresu IP klienta*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-789">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="4ed0b-790">Po dodaniu **adresu IP klienta** do listy dozwolone adresy IP kliknij pozycję **Zapisz** , aby potwierdzić zmiany.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-790">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Potwierdź zmiany](aspnet-mvc-4-fundamentals/_static/image59.png)

    <span data-ttu-id="4ed0b-792">*Potwierdź zmiany*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-792">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="4ed0b-793">Zadanie 3 — publikowanie aplikacji ASP.NET MVC 4 przy użyciu Web Deploy</span><span class="sxs-lookup"><span data-stu-id="4ed0b-793">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="4ed0b-794">Wróć do rozwiązania ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-794">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="4ed0b-795">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt witryny sieci Web i wybierz polecenie **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-795">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="4ed0b-796">![Publikowanie aplikacji](aspnet-mvc-4-fundamentals/_static/image60.png "Publikowanie aplikacji")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-796">![Publishing the Application](aspnet-mvc-4-fundamentals/_static/image60.png "Publishing the Application")</span></span>

    <span data-ttu-id="4ed0b-797">*Publikowanie witryny sieci Web*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-797">*Publishing the web site*</span></span>
2. <span data-ttu-id="4ed0b-798">Zaimportuj profil publikowania zapisany w pierwszym zadaniu.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-798">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="4ed0b-799">![Importowanie profilu publikowania](aspnet-mvc-4-fundamentals/_static/image61.png "Importowanie profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-799">![Importing the publish profile](aspnet-mvc-4-fundamentals/_static/image61.png "Importing the publish profile")</span></span>

    <span data-ttu-id="4ed0b-800">*Importowanie profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-800">*Importing publish profile*</span></span>
3. <span data-ttu-id="4ed0b-801">Kliknij pozycję **Weryfikuj połączenie**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-801">Click **Validate Connection**.</span></span> <span data-ttu-id="4ed0b-802">Po zakończeniu walidacji kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-802">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="4ed0b-803">Walidacja jest ukończona po wyświetleniu zielonego znacznika wyboru obok przycisku Weryfikuj połączenie.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-803">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="4ed0b-804">![Weryfikowanie połączenia](aspnet-mvc-4-fundamentals/_static/image62.png "Weryfikowanie połączenia")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-804">![Validating connection](aspnet-mvc-4-fundamentals/_static/image62.png "Validating connection")</span></span>

    <span data-ttu-id="4ed0b-805">*Weryfikowanie połączenia*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-805">*Validating connection*</span></span>
4. <span data-ttu-id="4ed0b-806">Na stronie **Ustawienia** w sekcji **bazy danych** kliknij przycisk obok pola tekstowego połączenie z bazą danych (np. **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="4ed0b-806">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="4ed0b-807">![Konfiguracja narzędzia Web Deploy](aspnet-mvc-4-fundamentals/_static/image63.png "Konfiguracja narzędzia Web Deploy")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-807">![Web deploy configuration](aspnet-mvc-4-fundamentals/_static/image63.png "Web deploy configuration")</span></span>

    <span data-ttu-id="4ed0b-808">*Konfiguracja narzędzia Web Deploy*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-808">*Web deploy configuration*</span></span>
5. <span data-ttu-id="4ed0b-809">Skonfiguruj połączenie z bazą danych w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="4ed0b-809">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="4ed0b-810">W polu **Nazwa serwera** wpisz adres URL serwera SQL Database przy użyciu prefiksu *TCP:* .</span><span class="sxs-lookup"><span data-stu-id="4ed0b-810">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="4ed0b-811">W polu **Nazwa użytkownika** wpisz nazwę logowania administratora serwera.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-811">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="4ed0b-812">W polu **hasło** wpisz hasło logowania administratora serwera.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-812">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="4ed0b-813">Wpisz nową nazwę bazy danych, na przykład: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-813">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="4ed0b-814">![Konfigurowanie parametrów połączenia docelowego](aspnet-mvc-4-fundamentals/_static/image64.png "Konfigurowanie parametrów połączenia docelowego")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-814">![Configuring destination connection string](aspnet-mvc-4-fundamentals/_static/image64.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="4ed0b-815">*Konfigurowanie parametrów połączenia docelowego*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-815">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="4ed0b-816">Następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-816">Then click **OK**.</span></span> <span data-ttu-id="4ed0b-817">Po wyświetleniu monitu o utworzenie bazy danych kliknij przycisk **tak**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-817">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="4ed0b-818">![Tworzenie bazy danych](aspnet-mvc-4-fundamentals/_static/image65.png "Tworzenie ciągu bazy danych")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-818">![Creating the database](aspnet-mvc-4-fundamentals/_static/image65.png "Creating the database string")</span></span>

    <span data-ttu-id="4ed0b-819">*Tworzenie bazy danych*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-819">*Creating the database*</span></span>
7. <span data-ttu-id="4ed0b-820">Parametry połączenia, które będą używane do nawiązywania połączenia z SQL Database w systemie Windows Azure, są wyświetlane w domyślnym polu tekstowym połączenie.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-820">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="4ed0b-821">Następnie kliknij przycisk **Next** (Dalej).</span><span class="sxs-lookup"><span data-stu-id="4ed0b-821">Then click **Next**.</span></span>

    <span data-ttu-id="4ed0b-822">![Parametry połączenia wskazujące SQL Database](aspnet-mvc-4-fundamentals/_static/image66.png "Parametry połączenia wskazujące SQL Database")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-822">![Connection string pointing to SQL Database](aspnet-mvc-4-fundamentals/_static/image66.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="4ed0b-823">*Parametry połączenia wskazujące SQL Database*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-823">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="4ed0b-824">Na stronie **Podgląd** kliknij przycisk **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-824">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="4ed0b-825">![Publikowanie aplikacji sieci Web](aspnet-mvc-4-fundamentals/_static/image67.png "Publikowanie aplikacji sieci Web")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-825">![Publishing the web application](aspnet-mvc-4-fundamentals/_static/image67.png "Publishing the web application")</span></span>

    <span data-ttu-id="4ed0b-826">*Publikowanie aplikacji sieci Web*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-826">*Publishing the web application*</span></span>
9. <span data-ttu-id="4ed0b-827">Po zakończeniu procesu publikowania w domyślnej przeglądarce zostanie otwarta opublikowana witryna sieci Web.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-827">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="4ed0b-828">![Aplikacja opublikowana w systemie Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "Aplikacja opublikowana w systemie Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-828">![Application published to Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="4ed0b-829">*Aplikacja opublikowana w systemie Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-829">*Application published to Windows Azure*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="4ed0b-830">Dodatek C: używanie fragmentów kodu</span><span class="sxs-lookup"><span data-stu-id="4ed0b-830">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="4ed0b-831">Za pomocą fragmentów kodu masz cały kod, którego potrzebujesz na wyręką.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-831">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="4ed0b-832">Dokument laboratoryjny powiedzie się dokładnie wtedy, gdy można z nich korzystać, jak pokazano na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-832">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="4ed0b-833">![Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio](aspnet-mvc-4-fundamentals/_static/image69.png "Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-833">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-fundamentals/_static/image69.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="4ed0b-834">*Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-834">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="4ed0b-835">***Aby dodać fragment kodu przy użyciu klawiatury (C# tylko)***</span><span class="sxs-lookup"><span data-stu-id="4ed0b-835">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="4ed0b-836">Umieść kursor, w którym chcesz wstawić kod.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-836">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="4ed0b-837">Zacznij wpisywać nazwę fragmentu kodu (bez spacji lub łączników).</span><span class="sxs-lookup"><span data-stu-id="4ed0b-837">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="4ed0b-838">Obejrzyj jako IntelliSense wyświetla pasujące nazwy fragmentów kodu.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-838">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="4ed0b-839">Wybierz poprawny fragment kodu (lub Kontynuuj wpisywanie, dopóki nie zostanie zaznaczona cała nazwa fragmentu kodu).</span><span class="sxs-lookup"><span data-stu-id="4ed0b-839">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="4ed0b-840">Naciśnij dwukrotnie klawisz Tab, aby wstawić fragment kodu w lokalizacji kursora.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-840">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="4ed0b-841">![Zacznij wpisywać nazwę fragmentu kodu](aspnet-mvc-4-fundamentals/_static/image70.png "Zacznij wpisywać nazwę fragmentu kodu")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-841">![Start typing the snippet name](aspnet-mvc-4-fundamentals/_static/image70.png "Start typing the snippet name")</span></span>

<span data-ttu-id="4ed0b-842">*Zacznij wpisywać nazwę fragmentu kodu*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-842">*Start typing the snippet name*</span></span>

<span data-ttu-id="4ed0b-843">![Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu](aspnet-mvc-4-fundamentals/_static/image71.png "Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-843">![Press Tab to select the highlighted snippet](aspnet-mvc-4-fundamentals/_static/image71.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="4ed0b-844">*Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-844">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="4ed0b-845">![Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta](aspnet-mvc-4-fundamentals/_static/image72.png "Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-845">![Press Tab again and the snippet will expand](aspnet-mvc-4-fundamentals/_static/image72.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="4ed0b-846">*Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-846">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="4ed0b-847">***Aby dodać fragment kodu przy użyciu myszy (C#, Visual Basic i XML)*** jedno.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-847">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="4ed0b-848">Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-848">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="4ed0b-849">Wybierz **Wstaw fragment** kodu, po którym następuje **Moje fragmenty kodów**.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-849">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="4ed0b-850">Wybierz odpowiedni fragment kodu z listy, klikając go.</span><span class="sxs-lookup"><span data-stu-id="4ed0b-850">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="4ed0b-851">![Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment](aspnet-mvc-4-fundamentals/_static/image73.png "Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-851">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-fundamentals/_static/image73.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="4ed0b-852">*Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-852">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="4ed0b-853">![Wybierz odpowiedni fragment kodu z listy, klikając go](aspnet-mvc-4-fundamentals/_static/image74.png "Wybierz odpowiedni fragment kodu z listy, klikając go")</span><span class="sxs-lookup"><span data-stu-id="4ed0b-853">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-fundamentals/_static/image74.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="4ed0b-854">*Wybierz odpowiedni fragment kodu z listy, klikając go*</span><span class="sxs-lookup"><span data-stu-id="4ed0b-854">*Pick the relevant snippet from the list, by clicking on it*</span></span>
