---
uid: web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
title: Testowanie jednostkowe ASP.NET Web API 2 | Microsoft Docs
author: Rick-Anderson
description: Te wskazówki i aplikacje pokazują, jak utworzyć proste testy jednostkowe dla aplikacji Web API 2. W tym samouczku przedstawiono sposób dołączania projektu testów jednostkowych...
ms.author: riande
ms.date: 06/05/2014
ms.assetid: bf20f78d-ff91-48be-abd1-88e23dcc70ba
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: f2d60b977475e048a3a74aabff4adc768ee22baf
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554972"
---
# <a name="unit-testing-aspnet-web-api-2"></a><span data-ttu-id="8b6ad-104">Testowanie jednostkowe ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="8b6ad-104">Unit Testing ASP.NET Web API 2</span></span>

<span data-ttu-id="8b6ad-105">Autor [FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="8b6ad-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[<span data-ttu-id="8b6ad-106">Pobierz ukończony projekt</span><span class="sxs-lookup"><span data-stu-id="8b6ad-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> <span data-ttu-id="8b6ad-107">Te wskazówki i aplikacje pokazują, jak utworzyć proste testy jednostkowe dla aplikacji Web API 2.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-107">This guidance and application demonstrate how to create simple unit tests for your Web API 2 application.</span></span> <span data-ttu-id="8b6ad-108">W tym samouczku pokazano, jak uwzględnić projekt testu jednostkowego w rozwiązaniu i pisać metody testowe, które sprawdzają zwrócone wartości z metody kontrolera.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-108">This tutorial shows how to include a unit test project in your solution, and write test methods that check the returned values from a controller method.</span></span>
>
> <span data-ttu-id="8b6ad-109">W tym samouczku założono, że znasz podstawowe pojęcia związane z interfejsem API sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-109">This tutorial assumes you are familiar with the basic concepts of ASP.NET Web API.</span></span> <span data-ttu-id="8b6ad-110">Aby zapoznać się z samouczkiem wprowadzającym, zobacz [wprowadzenie with Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="8b6ad-110">For an introductory tutorial, see [Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>
>
> <span data-ttu-id="8b6ad-111">Testy jednostkowe w tym temacie są celowo ograniczone do prostych scenariuszy danych.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-111">The unit tests in this topic are intentionally limited to simple data scenarios.</span></span> <span data-ttu-id="8b6ad-112">Aby przetestować jednostkowe bardziej zaawansowane scenariusze danych, zobacz [imitacja Entity Framework podczas testowania jednostkowego ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="8b6ad-112">For unit testing more advanced data scenarios, see [Mocking Entity Framework when Unit Testing ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="8b6ad-113">Wersje oprogramowania używane w samouczku</span><span class="sxs-lookup"><span data-stu-id="8b6ad-113">Software versions used in the tutorial</span></span>
>
> - [<span data-ttu-id="8b6ad-114">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="8b6ad-114">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - <span data-ttu-id="8b6ad-115">Internetowy interfejs API 2</span><span class="sxs-lookup"><span data-stu-id="8b6ad-115">Web API 2</span></span>

## <a name="in-this-topic"></a><span data-ttu-id="8b6ad-116">W tym temacie</span><span class="sxs-lookup"><span data-stu-id="8b6ad-116">In this topic</span></span>

<span data-ttu-id="8b6ad-117">Ten temat zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="8b6ad-117">This topic contains the following sections:</span></span>

- [<span data-ttu-id="8b6ad-118">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="8b6ad-118">Prerequisites</span></span>](#prereqs)
- [<span data-ttu-id="8b6ad-119">Pobierz kod</span><span class="sxs-lookup"><span data-stu-id="8b6ad-119">Download code</span></span>](#download)
- [<span data-ttu-id="8b6ad-120">Tworzenie aplikacji przy użyciu projektu testów jednostkowych</span><span class="sxs-lookup"><span data-stu-id="8b6ad-120">Create application with unit test project</span></span>](#appwithunittest)
    - [<span data-ttu-id="8b6ad-121">Dodaj projekt testu jednostkowego podczas tworzenia aplikacji</span><span class="sxs-lookup"><span data-stu-id="8b6ad-121">Add unit test project when creating the application</span></span>](#whencreate)
    - [<span data-ttu-id="8b6ad-122">Dodaj projekt testu jednostkowego do istniejącej aplikacji</span><span class="sxs-lookup"><span data-stu-id="8b6ad-122">Add unit test project to an existing application</span></span>](#addtoexisting)
- [<span data-ttu-id="8b6ad-123">Konfigurowanie aplikacji Web API 2</span><span class="sxs-lookup"><span data-stu-id="8b6ad-123">Set up the Web API 2 application</span></span>](#setupproject)
- [<span data-ttu-id="8b6ad-124">Zainstaluj pakiety NuGet w projekcie testowym</span><span class="sxs-lookup"><span data-stu-id="8b6ad-124">Install NuGet packages in test project</span></span>](#testpackages)
- [<span data-ttu-id="8b6ad-125">Utwórz testy</span><span class="sxs-lookup"><span data-stu-id="8b6ad-125">Create tests</span></span>](#tests)
- [<span data-ttu-id="8b6ad-126">Uruchom testy</span><span class="sxs-lookup"><span data-stu-id="8b6ad-126">Run tests</span></span>](#runtests)

<a id="prereqs"></a>
## <a name="prerequisites"></a><span data-ttu-id="8b6ad-127">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="8b6ad-127">Prerequisites</span></span>

<span data-ttu-id="8b6ad-128">[Program Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional lub Enterprise Edition</span><span class="sxs-lookup"><span data-stu-id="8b6ad-128">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional or Enterprise edition</span></span>

<a id="download"></a>
## <a name="download-code"></a><span data-ttu-id="8b6ad-129">Pobieranie kodu</span><span class="sxs-lookup"><span data-stu-id="8b6ad-129">Download code</span></span>

<span data-ttu-id="8b6ad-130">Pobierz [ukończony projekt](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span><span class="sxs-lookup"><span data-stu-id="8b6ad-130">Download the [completed project](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span></span> <span data-ttu-id="8b6ad-131">Projekt do pobrania zawiera kod testu jednostkowego dla tego tematu oraz dla [Entity Framework imitacji podczas testowania jednostek w sieci Web API ASP.NET](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) .</span><span class="sxs-lookup"><span data-stu-id="8b6ad-131">The downloadable project includes unit test code for this topic and for the [Mocking Entity Framework when Unit Testing ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) topic.</span></span>

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a><span data-ttu-id="8b6ad-132">Tworzenie aplikacji przy użyciu projektu testów jednostkowych</span><span class="sxs-lookup"><span data-stu-id="8b6ad-132">Create application with unit test project</span></span>

<span data-ttu-id="8b6ad-133">Możesz utworzyć projekt testu jednostkowego podczas tworzenia aplikacji lub dodać projekt testu jednostkowego do istniejącej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-133">You can either create a unit test project when creating your application or add a unit test project to an existing application.</span></span> <span data-ttu-id="8b6ad-134">W tym samouczku przedstawiono obie metody tworzenia projektu testów jednostkowych.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-134">This tutorial shows both methods for creating a unit test project.</span></span> <span data-ttu-id="8b6ad-135">Aby wykonać czynności opisane w tym samouczku, można użyć dowolnego z tych rozwiązań.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-135">To follow this tutorial, you can use either approach.</span></span>

<a id="whencreate"></a>
### <a name="add-unit-test-project-when-creating-the-application"></a><span data-ttu-id="8b6ad-136">Dodaj projekt testu jednostkowego podczas tworzenia aplikacji</span><span class="sxs-lookup"><span data-stu-id="8b6ad-136">Add unit test project when creating the application</span></span>

<span data-ttu-id="8b6ad-137">Utwórz nową aplikację sieci Web ASP.NET o nazwie **StoreApp**.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-137">Create a new ASP.NET Web Application named **StoreApp**.</span></span>

![Utwórz projekt](unit-testing-with-aspnet-web-api/_static/image1.png)

<span data-ttu-id="8b6ad-139">W oknie Nowy projekt ASP.NET okna, wybierz **pusty** szablon i Dodaj foldery i podstawowe odwołania dla internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-139">In the New ASP.NET Project windows, select the **Empty** template and add folders and core references for Web API.</span></span> <span data-ttu-id="8b6ad-140">Wybierz opcję **Dodaj testy jednostkowe** .</span><span class="sxs-lookup"><span data-stu-id="8b6ad-140">Select the **Add unit tests** option.</span></span> <span data-ttu-id="8b6ad-141">Projekt testu jednostkowego jest automatycznie nazwany **StoreApp. Tests**.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-141">The unit test project is automatically named **StoreApp.Tests**.</span></span> <span data-ttu-id="8b6ad-142">Możesz zachować tę nazwę.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-142">You can keep this name.</span></span>

![Utwórz projekt testu jednostkowego](unit-testing-with-aspnet-web-api/_static/image2.png)

<span data-ttu-id="8b6ad-144">Po utworzeniu aplikacji zobaczysz, że zawiera ona dwa projekty.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-144">After creating the application, you will see it contains two projects.</span></span>

![dwa projekty](unit-testing-with-aspnet-web-api/_static/image3.png)

<a id="addtoexisting"></a>
### <a name="add-unit-test-project-to-an-existing-application"></a><span data-ttu-id="8b6ad-146">Dodaj projekt testu jednostkowego do istniejącej aplikacji</span><span class="sxs-lookup"><span data-stu-id="8b6ad-146">Add unit test project to an existing application</span></span>

<span data-ttu-id="8b6ad-147">Jeśli projekt testu jednostkowego nie został utworzony podczas tworzenia aplikacji, możesz dodać go w dowolnym momencie.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-147">If you did not create the unit test project when you created your application, you can add one at any time.</span></span> <span data-ttu-id="8b6ad-148">Załóżmy na przykład, że masz już aplikację o nazwie StoreApp i chcesz dodać testy jednostkowe.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-148">For example, suppose you already have an application named StoreApp, and you want to add unit tests.</span></span> <span data-ttu-id="8b6ad-149">Aby dodać projekt testu jednostkowego, kliknij prawym przyciskiem myszy rozwiązanie, a następnie wybierz pozycję **Dodaj** i **Nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-149">To add a unit test project, right-click your solution and select **Add** and **New Project**.</span></span>

![Dodaj nowy projekt do rozwiązania](unit-testing-with-aspnet-web-api/_static/image4.png)

<span data-ttu-id="8b6ad-151">W lewym okienku wybierz pozycję **test** , a następnie wybierz pozycję **projekt testu jednostkowego** dla typu projektu.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-151">Select **Test** in the left pane, and select **Unit Test Project** for the project type.</span></span> <span data-ttu-id="8b6ad-152">Nazwij projekt **StoreApp. Tests**.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-152">Name the project **StoreApp.Tests**.</span></span>

![Dodaj projekt testu jednostkowego](unit-testing-with-aspnet-web-api/_static/image5.png)

<span data-ttu-id="8b6ad-154">W rozwiązaniu zobaczysz projekt testu jednostkowego.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-154">You will see the unit test project in your solution.</span></span>

<span data-ttu-id="8b6ad-155">W projekcie testów jednostkowych Dodaj odwołanie do projektu do oryginalnego projektu.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-155">In the unit test project, add a project reference to the original project.</span></span>

<a id="setupproject"></a>
## <a name="set-up-the-web-api-2-application"></a><span data-ttu-id="8b6ad-156">Konfigurowanie aplikacji Web API 2</span><span class="sxs-lookup"><span data-stu-id="8b6ad-156">Set up the Web API 2 application</span></span>

<span data-ttu-id="8b6ad-157">W projekcie StoreApp Dodaj plik klasy do folderu **models** o nazwie **Product.cs**.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-157">In your StoreApp project, add a class file to the **Models** folder named **Product.cs**.</span></span> <span data-ttu-id="8b6ad-158">Zastąp zawartość pliku następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-158">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="8b6ad-159">Skompiluj rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-159">Build the solution.</span></span>

<span data-ttu-id="8b6ad-160">Kliknij prawym przyciskiem myszy folder controllers, a następnie wybierz polecenie **Dodaj** i **nowy element szkieletowy**.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-160">Right-click the Controllers folder and select **Add** and **New Scaffolded Item**.</span></span> <span data-ttu-id="8b6ad-161">Wybierz **kontroler Web API 2 — pusty**.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-161">Select **Web API 2 Controller - Empty**.</span></span>

![Dodaj nowy kontroler](unit-testing-with-aspnet-web-api/_static/image6.png)

<span data-ttu-id="8b6ad-163">Ustaw nazwę kontrolera na **SimpleProductController**, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-163">Set the controller name to **SimpleProductController**, and click **Add**.</span></span>

![Określ kontroler](unit-testing-with-aspnet-web-api/_static/image7.png)

<span data-ttu-id="8b6ad-165">Zastąp istniejący kod następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-165">Replace the existing code with the following code.</span></span> <span data-ttu-id="8b6ad-166">Aby uprościć ten przykład, dane są przechowywane na liście, a nie w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-166">To simplify this example, the data is stored in a list rather than a database.</span></span> <span data-ttu-id="8b6ad-167">Lista zdefiniowana w tej klasie reprezentuje dane produkcyjne.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-167">The list defined in this class represents the production data.</span></span> <span data-ttu-id="8b6ad-168">Należy zauważyć, że kontroler zawiera konstruktora, który przyjmuje jako parametr listę obiektów produktów.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-168">Notice that the controller includes a constructor that takes as a parameter a list of Product objects.</span></span> <span data-ttu-id="8b6ad-169">Ten konstruktor umożliwia przekazywanie danych testowych podczas testowania jednostkowego.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-169">This constructor enables you to pass test data when unit testing.</span></span> <span data-ttu-id="8b6ad-170">Kontroler zawiera również dwie metody **asynchroniczne** do zilustrowania metod asynchronicznych testów jednostkowych.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-170">The controller also includes two **async** methods to illustrate unit testing asynchronous methods.</span></span> <span data-ttu-id="8b6ad-171">Te metody asynchroniczne zostały zaimplementowane przez wywołanie **Task. FromResult** , aby zminimalizować nadmiarowy kod, ale zazwyczaj metody obejmują operacje intensywnie korzystające z zasobów.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-171">These async methods were implemented by calling **Task.FromResult** to minimize extraneous code, but normally the methods would include resource-intensive operations.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="8b6ad-172">Metoda getproduct zwraca wystąpienie interfejsu **IHttpActionResult** .</span><span class="sxs-lookup"><span data-stu-id="8b6ad-172">The GetProduct method returns an instance of the **IHttpActionResult** interface.</span></span> <span data-ttu-id="8b6ad-173">IHttpActionResult to jedna z nowych funkcji w interfejsie Web API 2, która upraszcza proces tworzenia testów jednostkowych.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-173">IHttpActionResult is one of the new features in Web API 2, and it simplifies unit test development.</span></span> <span data-ttu-id="8b6ad-174">Klasy implementujące interfejs IHttpActionResult są dostępne w przestrzeni nazw [System. Web. http. results](https://msdn.microsoft.com/library/system.web.http.results.aspx) .</span><span class="sxs-lookup"><span data-stu-id="8b6ad-174">Classes that implement the IHttpActionResult interface are found in the [System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx) namespace.</span></span> <span data-ttu-id="8b6ad-175">Te klasy reprezentują możliwe odpowiedzi z żądania akcji i odpowiadają kodów stanu HTTP.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-175">These classes represent possible responses from an action request, and they correspond to HTTP status codes.</span></span>

<span data-ttu-id="8b6ad-176">Skompiluj rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-176">Build the solution.</span></span>

<span data-ttu-id="8b6ad-177">Teraz można przystąpić do konfigurowania projektu testowego.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-177">You are now ready to set up the test project.</span></span>

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a><span data-ttu-id="8b6ad-178">Zainstaluj pakiety NuGet w projekcie testowym</span><span class="sxs-lookup"><span data-stu-id="8b6ad-178">Install NuGet packages in test project</span></span>

<span data-ttu-id="8b6ad-179">W przypadku tworzenia aplikacji za pomocą pustego szablonu projekt testu jednostkowego (StoreApp. Tests) nie zawiera żadnych zainstalowanych pakietów NuGet.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-179">When you use the Empty template to create an application, the unit test project (StoreApp.Tests) does not include any installed NuGet packages.</span></span> <span data-ttu-id="8b6ad-180">Inne szablony, takie jak szablon internetowego interfejsu API, zawierają niektóre pakiety NuGet w projekcie testów jednostkowych.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-180">Other templates, such as the Web API template, include some NuGet packages in the unit test project.</span></span> <span data-ttu-id="8b6ad-181">W tym samouczku należy uwzględnić pakiet Microsoft ASP.NET Web API 2 Core w projekcie testowym.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-181">For this tutorial, you must include the Microsoft ASP.NET Web API 2 Core package to the test project.</span></span>

<span data-ttu-id="8b6ad-182">Kliknij prawym przyciskiem myszy projekt StoreApp. Tests i wybierz pozycję **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-182">Right-click the StoreApp.Tests project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="8b6ad-183">Musisz wybrać projekt StoreApp. Tests, aby dodać pakiety do tego projektu.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-183">You must select the StoreApp.Tests project to add the packages to that project.</span></span>

![Zarządzanie pakietami](unit-testing-with-aspnet-web-api/_static/image8.png)

<span data-ttu-id="8b6ad-185">Znajdź i zainstaluj pakiet Microsoft ASP.NET Web API 2 Core.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-185">Find and install Microsoft ASP.NET Web API 2 Core package.</span></span>

![Zainstaluj pakiet Core interfejsu API sieci Web](unit-testing-with-aspnet-web-api/_static/image9.png)

<span data-ttu-id="8b6ad-187">Zamknij okno Zarządzaj pakietami NuGet.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-187">Close the Manage NuGet Packages window.</span></span>

<a id="tests"></a>
## <a name="create-tests"></a><span data-ttu-id="8b6ad-188">Utwórz testy</span><span class="sxs-lookup"><span data-stu-id="8b6ad-188">Create tests</span></span>

<span data-ttu-id="8b6ad-189">Domyślnie projekt testowy zawiera pusty plik testowy o nazwie UnitTest1.cs.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-189">By default, your test project includes an empty test file named UnitTest1.cs.</span></span> <span data-ttu-id="8b6ad-190">Ten plik zawiera atrybuty używane do tworzenia metod testowych.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-190">This file shows the attributes you use to create test methods.</span></span> <span data-ttu-id="8b6ad-191">W przypadku testów jednostkowych możesz użyć tego pliku lub utworzyć własny plik.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-191">For your unit tests, you can either use this file or create your own file.</span></span>

![UnitTest1](unit-testing-with-aspnet-web-api/_static/image10.png)

<span data-ttu-id="8b6ad-193">Na potrzeby tego samouczka utworzysz własną klasę testową.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-193">For this tutorial, you will create your own test class.</span></span> <span data-ttu-id="8b6ad-194">Można usunąć plik UnitTest1.cs.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-194">You can delete the UnitTest1.cs file.</span></span> <span data-ttu-id="8b6ad-195">Dodaj klasę o nazwie **TestSimpleProductController.cs**i Zastąp kod następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-195">Add a class named **TestSimpleProductController.cs**, and replace the code with the following code.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample3.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a><span data-ttu-id="8b6ad-196">Uruchom testy</span><span class="sxs-lookup"><span data-stu-id="8b6ad-196">Run tests</span></span>

<span data-ttu-id="8b6ad-197">Teraz można przystąpić do uruchamiania testów.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-197">You are now ready to run the tests.</span></span> <span data-ttu-id="8b6ad-198">Zostanie przetestowana cała metoda oznaczona atrybutem **TestMethod** .</span><span class="sxs-lookup"><span data-stu-id="8b6ad-198">All of the method that are marked with the **TestMethod** attribute will be tested.</span></span> <span data-ttu-id="8b6ad-199">Z elementu menu **testowego** Uruchom testy.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-199">From the **Test** menu item, run the tests.</span></span>

![uruchom testy](unit-testing-with-aspnet-web-api/_static/image11.png)

<span data-ttu-id="8b6ad-201">Otwórz okno **Eksplorator testów** i zwróć uwagę na wyniki testów.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-201">Open the **Test Explorer** window, and notice the results of the tests.</span></span>

![wyniki testu](unit-testing-with-aspnet-web-api/_static/image12.png)

## <a name="summary"></a><span data-ttu-id="8b6ad-203">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="8b6ad-203">Summary</span></span>

<span data-ttu-id="8b6ad-204">Ten samouczek został ukończony.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-204">You have completed this tutorial.</span></span> <span data-ttu-id="8b6ad-205">Dane w tym samouczku zostały celowo uproszczone, aby skoncentrować się na warunkach testowania jednostkowego.</span><span class="sxs-lookup"><span data-stu-id="8b6ad-205">The data in this tutorial was intentionally simplified to focus on unit testing conditions.</span></span> <span data-ttu-id="8b6ad-206">Aby przetestować jednostkowe bardziej zaawansowane scenariusze danych, zobacz [imitacja Entity Framework podczas testowania jednostkowego ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="8b6ad-206">For unit testing more advanced data scenarios, see [Mocking Entity Framework when Unit Testing ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span></span>
