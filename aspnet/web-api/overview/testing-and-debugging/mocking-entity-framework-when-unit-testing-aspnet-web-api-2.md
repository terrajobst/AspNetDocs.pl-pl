---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: Imitacja Entity Framework, gdy testy jednostkowe ASP.NET Web API 2 | Microsoft Docs
author: Rick-Anderson
description: Te wskazówki i aplikacje pokazują, jak utworzyć testy jednostkowe dla aplikacji Web API 2, która używa Entity Framework. Pokazuje, jak zmodyfikować...
ms.author: riande
ms.date: 12/13/2013
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 258450107ee7443c4efd43a3b8e4851249745227
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78555091"
---
# <a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a><span data-ttu-id="6a9cf-104">Imitacja Entity Framework, gdy testy jednostkowe ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="6a9cf-104">Mocking Entity Framework when Unit Testing ASP.NET Web API 2</span></span>

<span data-ttu-id="6a9cf-105">Autor [FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="6a9cf-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[<span data-ttu-id="6a9cf-106">Pobierz ukończony projekt</span><span class="sxs-lookup"><span data-stu-id="6a9cf-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> <span data-ttu-id="6a9cf-107">Te wskazówki i aplikacje pokazują, jak utworzyć testy jednostkowe dla aplikacji Web API 2, która używa Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-107">This guidance and application demonstrate how to create unit tests for your Web API 2 application that uses the Entity Framework.</span></span> <span data-ttu-id="6a9cf-108">Przedstawiono w nim sposób modyfikacji kontrolera szkieletowego w celu umożliwienia przekazywania obiektu kontekstu do testowania oraz tworzenia obiektów testowych, które współpracują z Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-108">It shows how to modify the scaffolded controller to enable passing a context object for testing, and how to create test objects that work with Entity Framework.</span></span>
>
> <span data-ttu-id="6a9cf-109">Aby zapoznać się z wprowadzeniem do testów jednostkowych za pomocą interfejsu API sieci Web ASP.NET, zobacz [testowanie jednostkowe za pomocą ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="6a9cf-109">For an introduction to unit testing with ASP.NET Web API, see [Unit Testing with ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).</span></span>
>
> <span data-ttu-id="6a9cf-110">W tym samouczku założono, że znasz podstawowe pojęcia związane z interfejsem API sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-110">This tutorial assumes you are familiar with the basic concepts of ASP.NET Web API.</span></span> <span data-ttu-id="6a9cf-111">Aby zapoznać się z samouczkiem wprowadzającym, zobacz [wprowadzenie with Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="6a9cf-111">For an introductory tutorial, see [Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="6a9cf-112">Wersje oprogramowania używane w samouczku</span><span class="sxs-lookup"><span data-stu-id="6a9cf-112">Software versions used in the tutorial</span></span>
>
> - [<span data-ttu-id="6a9cf-113">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="6a9cf-113">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - <span data-ttu-id="6a9cf-114">Internetowy interfejs API 2</span><span class="sxs-lookup"><span data-stu-id="6a9cf-114">Web API 2</span></span>

## <a name="in-this-topic"></a><span data-ttu-id="6a9cf-115">W tym temacie</span><span class="sxs-lookup"><span data-stu-id="6a9cf-115">In this topic</span></span>

<span data-ttu-id="6a9cf-116">Ten temat zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="6a9cf-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="6a9cf-117">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="6a9cf-117">Prerequisites</span></span>](#prereqs)
- [<span data-ttu-id="6a9cf-118">Pobierz kod</span><span class="sxs-lookup"><span data-stu-id="6a9cf-118">Download code</span></span>](#download)
- [<span data-ttu-id="6a9cf-119">Tworzenie aplikacji przy użyciu projektu testów jednostkowych</span><span class="sxs-lookup"><span data-stu-id="6a9cf-119">Create application with unit test project</span></span>](#appwithunittest)
- [<span data-ttu-id="6a9cf-120">Tworzenie klasy modelu</span><span class="sxs-lookup"><span data-stu-id="6a9cf-120">Create the model class</span></span>](#modelclass)
- [<span data-ttu-id="6a9cf-121">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="6a9cf-121">Add the controller</span></span>](#controller)
- [<span data-ttu-id="6a9cf-122">Dodaj iniekcję zależności</span><span class="sxs-lookup"><span data-stu-id="6a9cf-122">Add dependency injection</span></span>](#dependency)
- [<span data-ttu-id="6a9cf-123">Zainstaluj pakiety NuGet w projekcie testowym</span><span class="sxs-lookup"><span data-stu-id="6a9cf-123">Install NuGet packages in test project</span></span>](#testpackages)
- [<span data-ttu-id="6a9cf-124">Utwórz kontekst testu</span><span class="sxs-lookup"><span data-stu-id="6a9cf-124">Create test context</span></span>](#testcontext)
- [<span data-ttu-id="6a9cf-125">Utwórz testy</span><span class="sxs-lookup"><span data-stu-id="6a9cf-125">Create tests</span></span>](#tests)
- [<span data-ttu-id="6a9cf-126">Uruchom testy</span><span class="sxs-lookup"><span data-stu-id="6a9cf-126">Run tests</span></span>](#runtests)

<span data-ttu-id="6a9cf-127">Jeśli wykonano już kroki [testowania jednostkowego w programie ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), możesz przejść do sekcji [Dodaj kontroler](#controller).</span><span class="sxs-lookup"><span data-stu-id="6a9cf-127">If you have already completed the steps in [Unit Testing with ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), you can skip to the section [Add the controller](#controller).</span></span>

<a id="prereqs"></a>
## <a name="prerequisites"></a><span data-ttu-id="6a9cf-128">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="6a9cf-128">Prerequisites</span></span>

<span data-ttu-id="6a9cf-129">Visual Studio 2017 Community, Professional lub Enterprise Edition</span><span class="sxs-lookup"><span data-stu-id="6a9cf-129">Visual Studio 2017 Community, Professional or Enterprise edition</span></span>

<a id="download"></a>
## <a name="download-code"></a><span data-ttu-id="6a9cf-130">Pobieranie kodu</span><span class="sxs-lookup"><span data-stu-id="6a9cf-130">Download code</span></span>

<span data-ttu-id="6a9cf-131">Pobierz [ukończony projekt](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span><span class="sxs-lookup"><span data-stu-id="6a9cf-131">Download the [completed project](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span></span> <span data-ttu-id="6a9cf-132">Projekt do pobrania zawiera kod testu jednostkowego dla tego tematu oraz dla [testów jednostkowych ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md) .</span><span class="sxs-lookup"><span data-stu-id="6a9cf-132">The downloadable project includes unit test code for this topic and for the [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md) topic.</span></span>

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a><span data-ttu-id="6a9cf-133">Tworzenie aplikacji przy użyciu projektu testów jednostkowych</span><span class="sxs-lookup"><span data-stu-id="6a9cf-133">Create application with unit test project</span></span>

<span data-ttu-id="6a9cf-134">Możesz utworzyć projekt testu jednostkowego podczas tworzenia aplikacji lub dodać projekt testu jednostkowego do istniejącej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-134">You can either create a unit test project when creating your application or add a unit test project to an existing application.</span></span> <span data-ttu-id="6a9cf-135">W tym samouczku przedstawiono tworzenie projektu testu jednostkowego podczas tworzenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-135">This tutorial shows creating a unit test project when creating the application.</span></span>

<span data-ttu-id="6a9cf-136">Utwórz nową aplikację sieci Web ASP.NET o nazwie **StoreApp**.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-136">Create a new ASP.NET Web Application named **StoreApp**.</span></span>

<span data-ttu-id="6a9cf-137">W oknie Nowy projekt ASP.NET okna, wybierz **pusty** szablon i Dodaj foldery i podstawowe odwołania dla internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-137">In the New ASP.NET Project windows, select the **Empty** template and add folders and core references for Web API.</span></span> <span data-ttu-id="6a9cf-138">Wybierz opcję **Dodaj testy jednostkowe** .</span><span class="sxs-lookup"><span data-stu-id="6a9cf-138">Select the **Add unit tests** option.</span></span> <span data-ttu-id="6a9cf-139">Projekt testu jednostkowego jest automatycznie nazwany **StoreApp. Tests**.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-139">The unit test project is automatically named **StoreApp.Tests**.</span></span> <span data-ttu-id="6a9cf-140">Możesz zachować tę nazwę.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-140">You can keep this name.</span></span>

![Utwórz projekt testu jednostkowego](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

<span data-ttu-id="6a9cf-142">Po utworzeniu aplikacji zobaczysz, że będzie ona zawierać dwa projekty — **StoreApp** i **StoreApp. Tests**.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-142">After creating the application, you will see it contains two projects - **StoreApp** and **StoreApp.Tests**.</span></span>

<a id="modelclass"></a>
## <a name="create-the-model-class"></a><span data-ttu-id="6a9cf-143">Tworzenie klasy modelu</span><span class="sxs-lookup"><span data-stu-id="6a9cf-143">Create the model class</span></span>

<span data-ttu-id="6a9cf-144">W projekcie StoreApp Dodaj plik klasy do folderu **models** o nazwie **Product.cs**.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-144">In your StoreApp project, add a class file to the **Models** folder named **Product.cs**.</span></span> <span data-ttu-id="6a9cf-145">Zastąp zawartość pliku następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-145">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

<span data-ttu-id="6a9cf-146">Skompiluj rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-146">Build the solution.</span></span>

<a id="controller"></a>
## <a name="add-the-controller"></a><span data-ttu-id="6a9cf-147">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="6a9cf-147">Add the controller</span></span>

<span data-ttu-id="6a9cf-148">Kliknij prawym przyciskiem myszy folder controllers, a następnie wybierz polecenie **Dodaj** i **nowy element szkieletowy**.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-148">Right-click the Controllers folder and select **Add** and **New Scaffolded Item**.</span></span> <span data-ttu-id="6a9cf-149">Wybierz kontroler Web API 2 z akcjami przy użyciu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-149">Select Web API 2 Controller with actions, using Entity Framework.</span></span>

![Dodaj nowy kontroler](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

<span data-ttu-id="6a9cf-151">Ustaw następujące wartości:</span><span class="sxs-lookup"><span data-stu-id="6a9cf-151">Set the following values:</span></span>

- <span data-ttu-id="6a9cf-152">Nazwa kontrolera: **ProductController**</span><span class="sxs-lookup"><span data-stu-id="6a9cf-152">Controller name: **ProductController**</span></span>
- <span data-ttu-id="6a9cf-153">Klasa modelu: **produkt**</span><span class="sxs-lookup"><span data-stu-id="6a9cf-153">Model class: **Product**</span></span>
- <span data-ttu-id="6a9cf-154">Klasa kontekstu danych: [Wybierz **Nowy przycisk kontekstu danych** , który wypełnia wartości widoczne poniżej]</span><span class="sxs-lookup"><span data-stu-id="6a9cf-154">Data context class: [Select **New data context** button which fills in the values seen below]</span></span>

![Określ kontroler](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

<span data-ttu-id="6a9cf-156">Kliknij przycisk **Dodaj** , aby utworzyć kontroler z automatycznie generowanym kodem.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-156">Click **Add** to create the controller with automatically-generated code.</span></span> <span data-ttu-id="6a9cf-157">Kod zawiera metody tworzenia, pobierania, aktualizowania i usuwania wystąpień klasy produktu.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-157">The code includes methods for creating, retrieving, updating and deleting instances of the Product class.</span></span> <span data-ttu-id="6a9cf-158">Poniższy kod przedstawia metodę dodawania produktu.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-158">The following code shows the method for add a Product.</span></span> <span data-ttu-id="6a9cf-159">Zwróć uwagę, że metoda zwraca wystąpienie elementu **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-159">Notice that the method returns an instance of **IHttpActionResult**.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

<span data-ttu-id="6a9cf-160">IHttpActionResult to jedna z nowych funkcji w interfejsie Web API 2, która upraszcza proces tworzenia testów jednostkowych.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-160">IHttpActionResult is one of the new features in Web API 2, and it simplifies unit test development.</span></span>

<span data-ttu-id="6a9cf-161">W następnej sekcji zostanie dostosowany wygenerowany kod, aby ułatwić przekazywanie obiektów testowych do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-161">In the next section, you will customize the generated code to facilitate passing test objects to the controller.</span></span>

<a id="dependency"></a>
## <a name="add-dependency-injection"></a><span data-ttu-id="6a9cf-162">Dodaj iniekcję zależności</span><span class="sxs-lookup"><span data-stu-id="6a9cf-162">Add dependency injection</span></span>

<span data-ttu-id="6a9cf-163">Obecnie Klasa ProductController jest zakodowana w taki sposób, aby korzystała z wystąpienia klasy StoreAppContext.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-163">Currently, the ProductController class is hard-coded to use an instance of the StoreAppContext class.</span></span> <span data-ttu-id="6a9cf-164">Użyjesz wzorca zwanego iniekcją zależności, aby zmodyfikować aplikację i usunąć tę sztywną zależność.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-164">You will use a pattern called dependency injection to modify your application and remove that hard-coded dependency.</span></span> <span data-ttu-id="6a9cf-165">Przerywając Tę zależność, można przekazać obiekt makiety podczas testowania.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-165">By breaking this dependency, you can pass in a mock object when testing.</span></span>

<span data-ttu-id="6a9cf-166">Kliknij prawym przyciskiem myszy folder **modele** i Dodaj nowy interfejs o nazwie **IStoreAppContext**.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-166">Right-click the **Models** folder, and add a new interface named **IStoreAppContext**.</span></span>

<span data-ttu-id="6a9cf-167">Zastąp kod następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-167">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

<span data-ttu-id="6a9cf-168">Otwórz plik StoreAppContext.cs i wprowadź następujące wyróżnione zmiany.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-168">Open the StoreAppContext.cs file and make the following highlighted changes.</span></span> <span data-ttu-id="6a9cf-169">Ważne zmiany należy zwrócić na następujące:</span><span class="sxs-lookup"><span data-stu-id="6a9cf-169">The important changes to note are:</span></span>

- <span data-ttu-id="6a9cf-170">Klasa StoreAppContext implementuje interfejs IStoreAppContext</span><span class="sxs-lookup"><span data-stu-id="6a9cf-170">StoreAppContext class implements IStoreAppContext interface</span></span>
- <span data-ttu-id="6a9cf-171">Zaimplementowano metodę MarkAsModified</span><span class="sxs-lookup"><span data-stu-id="6a9cf-171">MarkAsModified method is implemented</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

<span data-ttu-id="6a9cf-172">Otwórz plik ProductController.cs.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-172">Open the ProductController.cs file.</span></span> <span data-ttu-id="6a9cf-173">Zmień istniejący kod w taki sposób, aby był zgodny z wyróżnionym kodem.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-173">Change the existing code to match the highlighted code.</span></span> <span data-ttu-id="6a9cf-174">Te zmiany przerywają zależność od StoreAppContext i umożliwiają innym klasom przekazywanie w innym obiekcie dla klasy kontekstu.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-174">These changes break the dependency on StoreAppContext and enable other classes to pass in a different object for the context class.</span></span> <span data-ttu-id="6a9cf-175">Ta zmiana umożliwi przekazanie kontekstu testu podczas testów jednostkowych.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-175">This change will enable you to pass in a test context during unit tests.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

<span data-ttu-id="6a9cf-176">Istnieje jeszcze jedna zmiana, którą trzeba wykonać w ProductController.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-176">There is one more change you must make in ProductController.</span></span> <span data-ttu-id="6a9cf-177">W metodzie **PutProduct** Zastąp wiersz, który ustawia stan jednostki na zmodyfikowane przy użyciu wywołania metody MarkAsModified.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-177">In the **PutProduct** method, replace the line that sets the entity state to modified with a call to the MarkAsModified method.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

<span data-ttu-id="6a9cf-178">Skompiluj rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-178">Build the solution.</span></span>

<span data-ttu-id="6a9cf-179">Teraz można przystąpić do konfigurowania projektu testowego.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-179">You are now ready to set up the test project.</span></span>

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a><span data-ttu-id="6a9cf-180">Zainstaluj pakiety NuGet w projekcie testowym</span><span class="sxs-lookup"><span data-stu-id="6a9cf-180">Install NuGet packages in test project</span></span>

<span data-ttu-id="6a9cf-181">W przypadku tworzenia aplikacji za pomocą pustego szablonu projekt testu jednostkowego (StoreApp. Tests) nie zawiera żadnych zainstalowanych pakietów NuGet.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-181">When you use the Empty template to create an application, the unit test project (StoreApp.Tests) does not include any installed NuGet packages.</span></span> <span data-ttu-id="6a9cf-182">Inne szablony, takie jak szablon internetowego interfejsu API, zawierają niektóre pakiety NuGet w projekcie testów jednostkowych.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-182">Other templates, such as the Web API template, include some NuGet packages in the unit test project.</span></span> <span data-ttu-id="6a9cf-183">W tym samouczku należy uwzględnić pakiet Entity Framework i pakiet Microsoft ASP.NET Web API 2 Core w projekcie testowym.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-183">For this tutorial, you must include the Entity Framework package and the Microsoft ASP.NET Web API 2 Core package to the test project.</span></span>

<span data-ttu-id="6a9cf-184">Kliknij prawym przyciskiem myszy projekt StoreApp. Tests i wybierz pozycję **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-184">Right-click the StoreApp.Tests project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="6a9cf-185">Musisz wybrać projekt StoreApp. Tests, aby dodać pakiety do tego projektu.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-185">You must select the StoreApp.Tests project to add the packages to that project.</span></span>

![Zarządzanie pakietami](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

<span data-ttu-id="6a9cf-187">Z pakietów online Znajdź i zainstaluj pakiet EntityFramework (wersja 6,0 lub nowsza).</span><span class="sxs-lookup"><span data-stu-id="6a9cf-187">From the Online packages, find and install the EntityFramework package (version 6.0 or later).</span></span> <span data-ttu-id="6a9cf-188">Jeśli zostanie wyświetlona informacja, że pakiet EntityFramework jest już zainstalowany, prawdopodobnie zamiast projektu StoreApp. Tests został wybrany projekt StoreApp.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-188">If it appears that the EntityFramework package is already installed, you may have selected the StoreApp project instead of the StoreApp.Tests project.</span></span>

![Dodaj Entity Framework](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

<span data-ttu-id="6a9cf-190">Znajdź i zainstaluj pakiet Microsoft ASP.NET Web API 2 Core.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-190">Find and install Microsoft ASP.NET Web API 2 Core package.</span></span>

![Zainstaluj pakiet Core interfejsu API sieci Web](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

<span data-ttu-id="6a9cf-192">Zamknij okno Zarządzaj pakietami NuGet.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-192">Close the Manage NuGet Packages window.</span></span>

<a id="testcontext"></a>
## <a name="create-test-context"></a><span data-ttu-id="6a9cf-193">Utwórz kontekst testu</span><span class="sxs-lookup"><span data-stu-id="6a9cf-193">Create test context</span></span>

<span data-ttu-id="6a9cf-194">Dodaj klasę o nazwie **TestDbSet** do projektu testowego.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-194">Add a class named **TestDbSet** to the test project.</span></span> <span data-ttu-id="6a9cf-195">Ta klasa służy jako klasa bazowa dla zestawu danych testowych.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-195">This class serves as the base class for your test data set.</span></span> <span data-ttu-id="6a9cf-196">Zastąp kod następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-196">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample7.cs)]

<span data-ttu-id="6a9cf-197">Dodaj klasę o nazwie **TestProductDbSet** do projektu testowego, który zawiera poniższy kod.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-197">Add a class named **TestProductDbSet** to the test project which contains the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample8.cs)]

<span data-ttu-id="6a9cf-198">Dodaj klasę o nazwie **TestStoreAppContext** i Zastąp istniejący kod następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-198">Add a class named **TestStoreAppContext** and replace the existing code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample9.cs)]

<a id="tests"></a>
## <a name="create-tests"></a><span data-ttu-id="6a9cf-199">Utwórz testy</span><span class="sxs-lookup"><span data-stu-id="6a9cf-199">Create tests</span></span>

<span data-ttu-id="6a9cf-200">Domyślnie projekt testowy zawiera pusty plik testowy o nazwie **UnitTest1.cs**.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-200">By default, your test project includes an empty test file named **UnitTest1.cs**.</span></span> <span data-ttu-id="6a9cf-201">Ten plik zawiera atrybuty używane do tworzenia metod testowych.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-201">This file shows the attributes you use to create test methods.</span></span> <span data-ttu-id="6a9cf-202">Na potrzeby tego samouczka możesz usunąć ten plik, ponieważ dodasz nową klasę testową.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-202">For this tutorial, you can delete this file because you will add a new test class.</span></span>

<span data-ttu-id="6a9cf-203">Dodaj klasę o nazwie **TestProductController** do projektu testowego.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-203">Add a class named **TestProductController** to the test project.</span></span> <span data-ttu-id="6a9cf-204">Zastąp kod następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-204">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a><span data-ttu-id="6a9cf-205">Uruchom testy</span><span class="sxs-lookup"><span data-stu-id="6a9cf-205">Run tests</span></span>

<span data-ttu-id="6a9cf-206">Teraz można przystąpić do uruchamiania testów.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-206">You are now ready to run the tests.</span></span> <span data-ttu-id="6a9cf-207">Zostanie przetestowana cała metoda oznaczona atrybutem **TestMethod** .</span><span class="sxs-lookup"><span data-stu-id="6a9cf-207">All of the method that are marked with the **TestMethod** attribute will be tested.</span></span> <span data-ttu-id="6a9cf-208">Z elementu menu **testowego** Uruchom testy.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-208">From the **Test** menu item, run the tests.</span></span>

![uruchom testy](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

<span data-ttu-id="6a9cf-210">Otwórz okno **Eksplorator testów** i zwróć uwagę na wyniki testów.</span><span class="sxs-lookup"><span data-stu-id="6a9cf-210">Open the **Test Explorer** window, and notice the results of the tests.</span></span>

![wyniki testu](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)
