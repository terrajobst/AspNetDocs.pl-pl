---
uid: web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
title: ASP.NET Web API 2 testy jednostkowe | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Tej wskazówki i aplikacji przedstawiają sposób tworzenia testów jednostkowych proste dla aplikacji sieci Web API 2. W tym samouczku pokazano, jak dołączyć proj testu jednostki...
ms.author: riande
ms.date: 06/05/2014
ms.assetid: bf20f78d-ff91-48be-abd1-88e23dcc70ba
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 915610e6646ebe86dd8f16f290ecabd36bf7f48d
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57071966"
---
<a name="unit-testing-aspnet-web-api-2"></a><span data-ttu-id="d6444-104">ASP.NET Web API 2 testy jednostkowe</span><span class="sxs-lookup"><span data-stu-id="d6444-104">Unit Testing ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="d6444-105">przez [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d6444-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[<span data-ttu-id="d6444-106">Pobierz ukończony projekt</span><span class="sxs-lookup"><span data-stu-id="d6444-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> <span data-ttu-id="d6444-107">Tej wskazówki i aplikacji przedstawiają sposób tworzenia testów jednostkowych proste dla aplikacji sieci Web API 2.</span><span class="sxs-lookup"><span data-stu-id="d6444-107">This guidance and application demonstrate how to create simple unit tests for your Web API 2 application.</span></span> <span data-ttu-id="d6444-108">W tym samouczku pokazano, jak do uwzględnienia projekt testów jednostkowych w rozwiązaniu, a następnie zapisać metody testowe, sprawdzające wartości zwracane przez metodę kontrolera.</span><span class="sxs-lookup"><span data-stu-id="d6444-108">This tutorial shows how to include a unit test project in your solution, and write test methods that check the returned values from a controller method.</span></span>
>
> <span data-ttu-id="d6444-109">W tym samouczku przyjęto założenie, że znasz podstawowe pojęcia dotyczące środowiska ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="d6444-109">This tutorial assumes you are familiar with the basic concepts of ASP.NET Web API.</span></span> <span data-ttu-id="d6444-110">Aby uzyskać Samouczek wprowadzający, zobacz [wprowadzenie do wzorca ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="d6444-110">For an introductory tutorial, see [Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>
>
> <span data-ttu-id="d6444-111">Testy jednostkowe w tym temacie są celowo ograniczone do prostych danych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="d6444-111">The unit tests in this topic are intentionally limited to simple data scenarios.</span></span> <span data-ttu-id="d6444-112">Do testowania bardziej zaawansowanych scenariuszy danych jednostki, zobacz [pozorowanie programu Entity Framework podczas jednostki testowania wzorca ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="d6444-112">For unit testing more advanced data scenarios, see [Mocking Entity Framework when Unit Testing ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="d6444-113">Wersje oprogramowania używanego w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="d6444-113">Software versions used in the tutorial</span></span>
>
> - [<span data-ttu-id="d6444-114">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="d6444-114">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - <span data-ttu-id="d6444-115">Internetowy interfejs API 2</span><span class="sxs-lookup"><span data-stu-id="d6444-115">Web API 2</span></span>

## <a name="in-this-topic"></a><span data-ttu-id="d6444-116">W tym temacie:</span><span class="sxs-lookup"><span data-stu-id="d6444-116">In this topic</span></span>

<span data-ttu-id="d6444-117">Ten temat zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="d6444-117">This topic contains the following sections:</span></span>

- [<span data-ttu-id="d6444-118">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="d6444-118">Prerequisites</span></span>](#prereqs)
- [<span data-ttu-id="d6444-119">Pobierz program code</span><span class="sxs-lookup"><span data-stu-id="d6444-119">Download code</span></span>](#download)
- [<span data-ttu-id="d6444-120">Tworzenie aplikacji przy użyciu projektu testu jednostkowego</span><span class="sxs-lookup"><span data-stu-id="d6444-120">Create application with unit test project</span></span>](#appwithunittest)
    - [<span data-ttu-id="d6444-121">Dodaj projekt testu jednostkowego, podczas tworzenia aplikacji</span><span class="sxs-lookup"><span data-stu-id="d6444-121">Add unit test project when creating the application</span></span>](#whencreate)
    - [<span data-ttu-id="d6444-122">Dodaj projekt testu jednostkowego do istniejącej aplikacji</span><span class="sxs-lookup"><span data-stu-id="d6444-122">Add unit test project to an existing application</span></span>](#addtoexisting)
- [<span data-ttu-id="d6444-123">Konfigurowanie aplikacji sieci Web API 2</span><span class="sxs-lookup"><span data-stu-id="d6444-123">Set up the Web API 2 application</span></span>](#setupproject)
- [<span data-ttu-id="d6444-124">Instalowanie pakietów NuGet w projekcie testowym</span><span class="sxs-lookup"><span data-stu-id="d6444-124">Install NuGet packages in test project</span></span>](#testpackages)
- [<span data-ttu-id="d6444-125">Tworzenie testów</span><span class="sxs-lookup"><span data-stu-id="d6444-125">Create tests</span></span>](#tests)
- [<span data-ttu-id="d6444-126">Uruchom testy</span><span class="sxs-lookup"><span data-stu-id="d6444-126">Run tests</span></span>](#runtests)

<a id="prereqs"></a>
## <a name="prerequisites"></a><span data-ttu-id="d6444-127">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="d6444-127">Prerequisites</span></span>

<span data-ttu-id="d6444-128">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional or Enterprise edition</span><span class="sxs-lookup"><span data-stu-id="d6444-128">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional or Enterprise edition</span></span>

<a id="download"></a>
## <a name="download-code"></a><span data-ttu-id="d6444-129">Pobierz program code</span><span class="sxs-lookup"><span data-stu-id="d6444-129">Download code</span></span>

<span data-ttu-id="d6444-130">Pobierz [projektu ukończona](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span><span class="sxs-lookup"><span data-stu-id="d6444-130">Download the [completed project](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span></span> <span data-ttu-id="d6444-131">Projekt do pobrania zawiera kod testu jednostkowego dla tego tematu i [pozorowanie programu Entity Framework podczas jednostki testowania ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) tematu.</span><span class="sxs-lookup"><span data-stu-id="d6444-131">The downloadable project includes unit test code for this topic and for the [Mocking Entity Framework when Unit Testing ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) topic.</span></span>

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a><span data-ttu-id="d6444-132">Tworzenie aplikacji przy użyciu projektu testu jednostkowego</span><span class="sxs-lookup"><span data-stu-id="d6444-132">Create application with unit test project</span></span>

<span data-ttu-id="d6444-133">Możesz utworzyć projekt testów jednostkowych, podczas tworzenia aplikacji lub dodać projekt testów jednostkowych do istniejących aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d6444-133">You can either create a unit test project when creating your application or add a unit test project to an existing application.</span></span> <span data-ttu-id="d6444-134">Ten samouczek przedstawia tworzenie projektu testu jednostkowego obu metod.</span><span class="sxs-lookup"><span data-stu-id="d6444-134">This tutorial shows both methods for creating a unit test project.</span></span> <span data-ttu-id="d6444-135">Aby wykonać kroki tego samouczka, można użyć każda z tych metod.</span><span class="sxs-lookup"><span data-stu-id="d6444-135">To follow this tutorial, you can use either approach.</span></span>

<a id="whencreate"></a>
### <a name="add-unit-test-project-when-creating-the-application"></a><span data-ttu-id="d6444-136">Dodaj projekt testu jednostkowego, podczas tworzenia aplikacji</span><span class="sxs-lookup"><span data-stu-id="d6444-136">Add unit test project when creating the application</span></span>

<span data-ttu-id="d6444-137">Utwórz nową aplikację sieci Web programu ASP.NET o nazwie **StoreApp**.</span><span class="sxs-lookup"><span data-stu-id="d6444-137">Create a new ASP.NET Web Application named **StoreApp**.</span></span>

![Tworzenie projektu](unit-testing-with-aspnet-web-api/_static/image1.png)

<span data-ttu-id="d6444-139">W systemie windows nowy projekt ASP.NET, wybierz **pusty** szablon i Dodaj foldery i podstawowe odwołania dla internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="d6444-139">In the New ASP.NET Project windows, select the **Empty** template and add folders and core references for Web API.</span></span> <span data-ttu-id="d6444-140">Wybierz **dodać testy jednostkowe** opcji.</span><span class="sxs-lookup"><span data-stu-id="d6444-140">Select the **Add unit tests** option.</span></span> <span data-ttu-id="d6444-141">Projekt testów jednostkowych ma automatycznie nadawaną nazwę **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="d6444-141">The unit test project is automatically named **StoreApp.Tests**.</span></span> <span data-ttu-id="d6444-142">Możesz zachować tę nazwę.</span><span class="sxs-lookup"><span data-stu-id="d6444-142">You can keep this name.</span></span>

![Tworzenie projektu testu jednostkowego](unit-testing-with-aspnet-web-api/_static/image2.png)

<span data-ttu-id="d6444-144">Po utworzeniu aplikacji, zobaczysz, że zawiera dwa projekty.</span><span class="sxs-lookup"><span data-stu-id="d6444-144">After creating the application, you will see it contains two projects.</span></span>

![dwa projekty](unit-testing-with-aspnet-web-api/_static/image3.png)

<a id="addtoexisting"></a>
### <a name="add-unit-test-project-to-an-existing-application"></a><span data-ttu-id="d6444-146">Dodaj projekt testu jednostkowego do istniejącej aplikacji</span><span class="sxs-lookup"><span data-stu-id="d6444-146">Add unit test project to an existing application</span></span>

<span data-ttu-id="d6444-147">Jeśli nie utworzono projekt testów jednostkowych podczas tworzenia aplikacji, możesz dodać jeden w dowolnym momencie.</span><span class="sxs-lookup"><span data-stu-id="d6444-147">If you did not create the unit test project when you created your application, you can add one at any time.</span></span> <span data-ttu-id="d6444-148">Na przykład, załóżmy, że masz już aplikację o nazwie StoreApp i chcesz dodać testy jednostkowe.</span><span class="sxs-lookup"><span data-stu-id="d6444-148">For example, suppose you already have an application named StoreApp, and you want to add unit tests.</span></span> <span data-ttu-id="d6444-149">Aby dodać projekt testu jednostkowego, kliknij prawym przyciskiem myszy rozwiązanie, a następnie wybierz **Dodaj** i **nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="d6444-149">To add a unit test project, right-click your solution and select **Add** and **New Project**.</span></span>

![Dodaj nowy projekt do rozwiązania](unit-testing-with-aspnet-web-api/_static/image4.png)

<span data-ttu-id="d6444-151">Wybierz **testu** w okienku po lewej stronie i wybierz **projektu testu jednostkowego** dla typu projektu.</span><span class="sxs-lookup"><span data-stu-id="d6444-151">Select **Test** in the left pane, and select **Unit Test Project** for the project type.</span></span> <span data-ttu-id="d6444-152">Nadaj projektowi nazwę **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="d6444-152">Name the project **StoreApp.Tests**.</span></span>

![Dodaj projekt testu jednostkowego](unit-testing-with-aspnet-web-api/_static/image5.png)

<span data-ttu-id="d6444-154">Zobaczysz projekt testów jednostkowych w rozwiązaniu.</span><span class="sxs-lookup"><span data-stu-id="d6444-154">You will see the unit test project in your solution.</span></span>

<span data-ttu-id="d6444-155">W projekcie testu jednostki Dodaj odwołanie do oryginalnego projektu.</span><span class="sxs-lookup"><span data-stu-id="d6444-155">In the unit test project, add a project reference to the original project.</span></span>

<a id="setupproject"></a>
## <a name="set-up-the-web-api-2-application"></a><span data-ttu-id="d6444-156">Konfigurowanie aplikacji sieci Web API 2</span><span class="sxs-lookup"><span data-stu-id="d6444-156">Set up the Web API 2 application</span></span>

<span data-ttu-id="d6444-157">W projekcie StoreApp Dodaj plik klasy do **modeli** folder o nazwie **Product.cs**.</span><span class="sxs-lookup"><span data-stu-id="d6444-157">In your StoreApp project, add a class file to the **Models** folder named **Product.cs**.</span></span> <span data-ttu-id="d6444-158">Zastąp zawartość pliku następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="d6444-158">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="d6444-159">Skompiluj rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="d6444-159">Build the solution.</span></span>

<span data-ttu-id="d6444-160">Kliknij prawym przyciskiem myszy folder kontrolerów, a następnie wybierz pozycję **Dodaj** i **nowy element szkieletu**.</span><span class="sxs-lookup"><span data-stu-id="d6444-160">Right-click the Controllers folder and select **Add** and **New Scaffolded Item**.</span></span> <span data-ttu-id="d6444-161">Wybierz **pusty kontroler - Web API 2**.</span><span class="sxs-lookup"><span data-stu-id="d6444-161">Select **Web API 2 Controller - Empty**.</span></span>

![Dodaj nowy kontroler](unit-testing-with-aspnet-web-api/_static/image6.png)

<span data-ttu-id="d6444-163">Ustaw nazwę kontrolera **SimpleProductController**i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="d6444-163">Set the controller name to **SimpleProductController**, and click **Add**.</span></span>

![Określ kontroler](unit-testing-with-aspnet-web-api/_static/image7.png)

<span data-ttu-id="d6444-165">Zastąp istniejący kod następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="d6444-165">Replace the existing code with the following code.</span></span> <span data-ttu-id="d6444-166">Aby uprościć ten przykład, dane są przechowywane w listy, a nie bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d6444-166">To simplify this example, the data is stored in a list rather than a database.</span></span> <span data-ttu-id="d6444-167">Listy zdefiniowanej w tej klasie reprezentuje danych produkcyjnych.</span><span class="sxs-lookup"><span data-stu-id="d6444-167">The list defined in this class represents the production data.</span></span> <span data-ttu-id="d6444-168">Zwróć uwagę, że kontroler konstruktora, który przyjmuje jako parametr listę obiektów produktu.</span><span class="sxs-lookup"><span data-stu-id="d6444-168">Notice that the controller includes a constructor that takes as a parameter a list of Product objects.</span></span> <span data-ttu-id="d6444-169">Ten konstruktor umożliwia przekazywanie danych testowych podczas testów jednostkowych.</span><span class="sxs-lookup"><span data-stu-id="d6444-169">This constructor enables you to pass test data when unit testing.</span></span> <span data-ttu-id="d6444-170">Kontroler obejmuje również dwa **async** metody, aby zilustrować metody asynchroniczne testy jednostkowe.</span><span class="sxs-lookup"><span data-stu-id="d6444-170">The controller also includes two **async** methods to illustrate unit testing asynchronous methods.</span></span> <span data-ttu-id="d6444-171">Te metody asynchroniczne zostały zaimplementowane przez wywołanie metody **Task.FromResult** zminimalizować nadmiarowe kod, ale zazwyczaj metody obejmuje operacje dużej ilości zasobów.</span><span class="sxs-lookup"><span data-stu-id="d6444-171">These async methods were implemented by calling **Task.FromResult** to minimize extraneous code, but normally the methods would include resource-intensive operations.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="d6444-172">Metoda GetProduct Zwraca wystąpienie **IHttpActionResult** interfejsu.</span><span class="sxs-lookup"><span data-stu-id="d6444-172">The GetProduct method returns an instance of the **IHttpActionResult** interface.</span></span> <span data-ttu-id="d6444-173">IHttpActionResult jest jedną z nowych funkcji w sieci Web API 2 i upraszcza tworzenie testów jednostkowych.</span><span class="sxs-lookup"><span data-stu-id="d6444-173">IHttpActionResult is one of the new features in Web API 2, and it simplifies unit test development.</span></span> <span data-ttu-id="d6444-174">Klasy, które implementują interfejs IHttpActionResult znajdują się w [System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx) przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="d6444-174">Classes that implement the IHttpActionResult interface are found in the [System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx) namespace.</span></span> <span data-ttu-id="d6444-175">Te klasy reprezentują możliwe odpowiedzi z akcji żądania, a odpowiadają one kodów stanu HTTP.</span><span class="sxs-lookup"><span data-stu-id="d6444-175">These classes represent possible responses from an action request, and they correspond to HTTP status codes.</span></span>

<span data-ttu-id="d6444-176">Skompiluj rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="d6444-176">Build the solution.</span></span>

<span data-ttu-id="d6444-177">Teraz można przystąpić do konfigurowania projektu testowego.</span><span class="sxs-lookup"><span data-stu-id="d6444-177">You are now ready to set up the test project.</span></span>

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a><span data-ttu-id="d6444-178">Instalowanie pakietów NuGet w projekcie testowym</span><span class="sxs-lookup"><span data-stu-id="d6444-178">Install NuGet packages in test project</span></span>

<span data-ttu-id="d6444-179">Gdy używasz pusty szablon do tworzenia aplikacji projektu testu jednostkowego (StoreApp.Tests) nie obejmuje wszystkie zainstalowane pakiety NuGet.</span><span class="sxs-lookup"><span data-stu-id="d6444-179">When you use the Empty template to create an application, the unit test project (StoreApp.Tests) does not include any installed NuGet packages.</span></span> <span data-ttu-id="d6444-180">Inne szablony, takie jak szablon interfejsu API sieci Web obejmują niektóre pakiety NuGet w projekcie testów jednostkowych.</span><span class="sxs-lookup"><span data-stu-id="d6444-180">Other templates, such as the Web API template, include some NuGet packages in the unit test project.</span></span> <span data-ttu-id="d6444-181">W tym samouczku musi zawierać pakiet Microsoft ASP.NET Web API 2 Core do projektu testowego.</span><span class="sxs-lookup"><span data-stu-id="d6444-181">For this tutorial, you must include the Microsoft ASP.NET Web API 2 Core package to the test project.</span></span>

<span data-ttu-id="d6444-182">Kliknij prawym przyciskiem myszy projekt StoreApp.Tests, a następnie wybierz pozycję **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="d6444-182">Right-click the StoreApp.Tests project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="d6444-183">Musisz wybrać projekt StoreApp.Tests, aby dodać pakiety do tego projektu.</span><span class="sxs-lookup"><span data-stu-id="d6444-183">You must select the StoreApp.Tests project to add the packages to that project.</span></span>

![Zarządzanie pakietami](unit-testing-with-aspnet-web-api/_static/image8.png)

<span data-ttu-id="d6444-185">Znajdź i zainstaluj pakiet Microsoft ASP.NET Web API 2 Core.</span><span class="sxs-lookup"><span data-stu-id="d6444-185">Find and install Microsoft ASP.NET Web API 2 Core package.</span></span>

![Zainstaluj pakiet podstawowego interfejsu api sieci web](unit-testing-with-aspnet-web-api/_static/image9.png)

<span data-ttu-id="d6444-187">Zamknij okno Zarządzanie pakietami NuGet.</span><span class="sxs-lookup"><span data-stu-id="d6444-187">Close the Manage NuGet Packages window.</span></span>

<a id="tests"></a>
## <a name="create-tests"></a><span data-ttu-id="d6444-188">Tworzenie testów</span><span class="sxs-lookup"><span data-stu-id="d6444-188">Create tests</span></span>

<span data-ttu-id="d6444-189">Domyślnie projektu testowego zawiera plik pusty test o nazwie UnitTest1.cs.</span><span class="sxs-lookup"><span data-stu-id="d6444-189">By default, your test project includes an empty test file named UnitTest1.cs.</span></span> <span data-ttu-id="d6444-190">Ten plik zawiera atrybuty, że umożliwia tworzenie metod testowych.</span><span class="sxs-lookup"><span data-stu-id="d6444-190">This file shows the attributes you use to create test methods.</span></span> <span data-ttu-id="d6444-191">Dla testów jednostkowych można użyć tego pliku lub Utwórz własny plik.</span><span class="sxs-lookup"><span data-stu-id="d6444-191">For your unit tests, you can either use this file or create your own file.</span></span>

![UnitTest1](unit-testing-with-aspnet-web-api/_static/image10.png)

<span data-ttu-id="d6444-193">W tym samouczku utworzysz klasy testowej.</span><span class="sxs-lookup"><span data-stu-id="d6444-193">For this tutorial, you will create your own test class.</span></span> <span data-ttu-id="d6444-194">Możesz usunąć plik UnitTest1.cs.</span><span class="sxs-lookup"><span data-stu-id="d6444-194">You can delete the UnitTest1.cs file.</span></span> <span data-ttu-id="d6444-195">Dodaj klasę o nazwie **TestSimpleProductController.cs**i Zastąp kod następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="d6444-195">Add a class named **TestSimpleProductController.cs**, and replace the code with the following code.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample3.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a><span data-ttu-id="d6444-196">Uruchom testy</span><span class="sxs-lookup"><span data-stu-id="d6444-196">Run tests</span></span>

<span data-ttu-id="d6444-197">Teraz można przystąpić do uruchomienia testów.</span><span class="sxs-lookup"><span data-stu-id="d6444-197">You are now ready to run the tests.</span></span> <span data-ttu-id="d6444-198">Wszystkie metody, które są oznaczone **TestMethod** atrybut, który będzie testowany.</span><span class="sxs-lookup"><span data-stu-id="d6444-198">All of the method that are marked with the **TestMethod** attribute will be tested.</span></span> <span data-ttu-id="d6444-199">Z **testu** elementu menu, uruchom testy.</span><span class="sxs-lookup"><span data-stu-id="d6444-199">From the **Test** menu item, run the tests.</span></span>

![uruchom testy](unit-testing-with-aspnet-web-api/_static/image11.png)

<span data-ttu-id="d6444-201">Otwórz **Eksplorator testów** okna i zwróć uwagę, wyniki testów.</span><span class="sxs-lookup"><span data-stu-id="d6444-201">Open the **Test Explorer** window, and notice the results of the tests.</span></span>

![wyniki testu](unit-testing-with-aspnet-web-api/_static/image12.png)

## <a name="summary"></a><span data-ttu-id="d6444-203">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="d6444-203">Summary</span></span>

<span data-ttu-id="d6444-204">W tym samouczku została ukończona.</span><span class="sxs-lookup"><span data-stu-id="d6444-204">You have completed this tutorial.</span></span> <span data-ttu-id="d6444-205">Danych w ramach tego samouczka został celowo uproszczona skoncentrować się na warunki testów jednostkowych.</span><span class="sxs-lookup"><span data-stu-id="d6444-205">The data in this tutorial was intentionally simplified to focus on unit testing conditions.</span></span> <span data-ttu-id="d6444-206">Do testowania bardziej zaawansowanych scenariuszy danych jednostki, zobacz [pozorowanie programu Entity Framework podczas jednostki testowania wzorca ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="d6444-206">For unit testing more advanced data scenarios, see [Mocking Entity Framework when Unit Testing ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span></span>
