---
title: Tworzenie pierwszej aplikacji składniki Razor
author: guardrex
description: Tworzenie składników Razor aplikacji krok po kroku i pojęcia dotyczące podstawowych składników Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/11/2019
uid: tutorials/first-razor-components-app
ms.openlocfilehash: 0c3dd2366581d73bad44e2911602e13c6c0daf9a
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57070871"
---
# <a name="build-your-first-razor-components-app"></a><span data-ttu-id="7152f-103">Tworzenie pierwszej aplikacji składniki Razor</span><span class="sxs-lookup"><span data-stu-id="7152f-103">Build your first Razor Components app</span></span>

<span data-ttu-id="7152f-104">Przez [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="7152f-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="7152f-105">W tym samouczku pokazano, jak utworzyć aplikację ze składnikami Razor i pokazuje podstawowe pojęcia składniki Razor.</span><span class="sxs-lookup"><span data-stu-id="7152f-105">This tutorial shows you how to build an app with Razor Components and demonstrates basic Razor Components concepts.</span></span> <span data-ttu-id="7152f-106">Można korzystać z tego samouczka przy użyciu obu składników Razor na podstawie projektu (obsługiwane w programie .NET Core 3.0 lub nowszej) lub projektu na podstawie Blazor (obsługiwane w przyszłej wersji programu .NET Core).</span><span class="sxs-lookup"><span data-stu-id="7152f-106">You can enjoy this tutorial using either a Razor Components-based project (supported in .NET Core 3.0 or later) or using a Blazor-based project (supported in a future release of .NET Core).</span></span>

<span data-ttu-id="7152f-107">Środowisko za pomocą platformy ASP.NET Core Razor składników (*zalecane*):</span><span class="sxs-lookup"><span data-stu-id="7152f-107">For an experience using ASP.NET Core Razor Components (*recommended*):</span></span>

* <span data-ttu-id="7152f-108">Postępuj zgodnie ze wskazówkami w <xref:razor-components/get-started> do tworzenia projektu na podstawie składników Razor.</span><span class="sxs-lookup"><span data-stu-id="7152f-108">Follow the guidance in <xref:razor-components/get-started> to create a Razor Components-based project.</span></span>
* <span data-ttu-id="7152f-109">Nadaj projektowi nazwę `RazorComponents`.</span><span class="sxs-lookup"><span data-stu-id="7152f-109">Name the project `RazorComponents`.</span></span>
* <span data-ttu-id="7152f-110">Rozwiązanie z wieloma projektami jest tworzona z szablonu Razor składników.</span><span class="sxs-lookup"><span data-stu-id="7152f-110">A multi-project solution is created from the Razor Components template.</span></span> <span data-ttu-id="7152f-111">Projekt Razor składników zostanie wygenerowany jako *RazorComponents.App*.</span><span class="sxs-lookup"><span data-stu-id="7152f-111">The Razor Components project is generated as *RazorComponents.App*.</span></span>

<span data-ttu-id="7152f-112">Aby uzyskać doświadczenie w korzystaniu z Blazor:</span><span class="sxs-lookup"><span data-stu-id="7152f-112">For an experience using Blazor:</span></span>

* <span data-ttu-id="7152f-113">Postępuj zgodnie ze wskazówkami w <xref:spa/blazor/get-started> do tworzenia projektu na podstawie Blazor.</span><span class="sxs-lookup"><span data-stu-id="7152f-113">Follow the guidance in <xref:spa/blazor/get-started> to create a Blazor-based project.</span></span>
* <span data-ttu-id="7152f-114">Nadaj projektowi nazwę `Blazor`.</span><span class="sxs-lookup"><span data-stu-id="7152f-114">Name the project `Blazor`.</span></span>
* <span data-ttu-id="7152f-115">Rozwiązania jednego projektu jest tworzona z szablonu Blazor.</span><span class="sxs-lookup"><span data-stu-id="7152f-115">A single-project solution is created from the Blazor template.</span></span>

<span data-ttu-id="7152f-116">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="7152f-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="7152f-117">Zobacz następujące tematy dotyczące wymagań wstępnych:</span><span class="sxs-lookup"><span data-stu-id="7152f-117">See the following topics for prerequisites:</span></span>

## <a name="build-components"></a><span data-ttu-id="7152f-118">Tworzenie składników</span><span class="sxs-lookup"><span data-stu-id="7152f-118">Build components</span></span>

1. <span data-ttu-id="7152f-119">Przejdź do wszystkich aplikacji trzy strony: Strona główna licznik i pobierania danych.</span><span class="sxs-lookup"><span data-stu-id="7152f-119">Browse to each of the app's three pages: Home, Counter, and Fetch data.</span></span> <span data-ttu-id="7152f-120">Te strony są implementowane przez pliki Razor w *stron* folderu: *Index.cshtml*, *Counter.cshtml*, i *FetchData.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="7152f-120">These pages are implemented by Razor files in the *Pages* folder: *Index.cshtml*, *Counter.cshtml*, and *FetchData.cshtml*.</span></span>

1. <span data-ttu-id="7152f-121">Na stronie licznika wybierz **kliknij mnie** przycisk, aby zwiększyć licznik bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="7152f-121">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="7152f-122">Zwiększenie licznika, na stronie sieci Web zwykle wtedy konieczne napisanie kodu JavaScript, ale składniki Razor zapewnia lepsze przy użyciu podejścia C#.</span><span class="sxs-lookup"><span data-stu-id="7152f-122">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor Components provides a better approach using C#.</span></span>

1. <span data-ttu-id="7152f-123">Zapoznania się z implementacją składnikiem licznika w *Counter.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="7152f-123">Examine the implementation of the Counter component in the *Counter.cshtml* file.</span></span>

   <span data-ttu-id="7152f-124">*Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="7152f-124">*Pages/Counter.cshtml*:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter1.cshtml)]

   <span data-ttu-id="7152f-125">Interfejs użytkownika składnika licznika jest zdefiniowana za pomocą kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="7152f-125">The UI of the Counter component is defined using HTML.</span></span> <span data-ttu-id="7152f-126">Logika renderowania dynamicznego (na przykład pętli, warunkowych, wyrażeń) zostanie dodany przy użyciu osadzonych C# składni o nazwie [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="7152f-126">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="7152f-127">Kod znaczników HTML i C# logiki renderowania są konwertowane na klasę składnika w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="7152f-127">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="7152f-128">Nazwa wygenerowanej klasy .NET zgodny z nazwą pliku.</span><span class="sxs-lookup"><span data-stu-id="7152f-128">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="7152f-129">Elementy członkowskie klasy składników są zdefiniowane w `@functions` bloku.</span><span class="sxs-lookup"><span data-stu-id="7152f-129">Members of the component class are defined in a `@functions` block.</span></span> <span data-ttu-id="7152f-130">W `@functions` blokować stan składnika (właściwości, pola) i metod są określone dla obsługi zdarzeń lub Definiowanie logiki innych składników.</span><span class="sxs-lookup"><span data-stu-id="7152f-130">In the `@functions` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="7152f-131">Te elementy członkowskie są następnie używane w ramach składnika renderowania logiki oraz obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="7152f-131">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="7152f-132">Gdy **kliknij mnie** wybrany przycisk:</span><span class="sxs-lookup"><span data-stu-id="7152f-132">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="7152f-133">Zarejestrowane składnik licznika `onclick` program obsługi jest wywoływany ( `IncrementCount` metody).</span><span class="sxs-lookup"><span data-stu-id="7152f-133">The Counter component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="7152f-134">Składnik licznika generuje drzewo jego renderowania.</span><span class="sxs-lookup"><span data-stu-id="7152f-134">The Counter component regenerates its render tree.</span></span>
   * <span data-ttu-id="7152f-135">Nowe drzewo renderowania w porównaniu do poprzedniego.</span><span class="sxs-lookup"><span data-stu-id="7152f-135">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="7152f-136">Modyfikacje do modelu DOM (Document Object) są stosowane.</span><span class="sxs-lookup"><span data-stu-id="7152f-136">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="7152f-137">Liczba wyświetlanych jest aktualizowana.</span><span class="sxs-lookup"><span data-stu-id="7152f-137">The displayed count is updated.</span></span>

1. <span data-ttu-id="7152f-138">Modyfikowanie C# logiki licznika składnika zwiększenie liczby dwa, a nie jeden.</span><span class="sxs-lookup"><span data-stu-id="7152f-138">Modify the C# logic of the Counter component to make the count increment by two instead of one.</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter2.cshtml?highlight=14)]

1. <span data-ttu-id="7152f-139">Ponownie skompiluj i uruchom aplikację, aby zobaczyć zmiany.</span><span class="sxs-lookup"><span data-stu-id="7152f-139">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="7152f-140">Wybierz **kliknij mnie** przycisk i zwiększa licznik przez dwa.</span><span class="sxs-lookup"><span data-stu-id="7152f-140">Select the **Click me** button, and the counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="7152f-141">Używanie składników</span><span class="sxs-lookup"><span data-stu-id="7152f-141">Use components</span></span>

<span data-ttu-id="7152f-142">Uwzględnij składnika w innym składniku przy użyciu składni notacji HTML.</span><span class="sxs-lookup"><span data-stu-id="7152f-142">Include a component into another component using an HTML-like syntax.</span></span>

1. <span data-ttu-id="7152f-143">Dodaj składnik licznika do aplikacji, składnik indeksu (strona główna), dodając `<Counter />` elementu składnik indeksu.</span><span class="sxs-lookup"><span data-stu-id="7152f-143">Add the Counter component to the app's Index (home page) component by adding a `<Counter />` element to the Index component.</span></span>

   <span data-ttu-id="7152f-144">Jeśli używasz Blazor dla tego środowiska, składnik ankiety monitu (`<SurveyPrompt>` elementu) znajduje się w składnik indeksu.</span><span class="sxs-lookup"><span data-stu-id="7152f-144">If you're using Blazor for this experience, a Survey Prompt component (`<SurveyPrompt>` element) is in the Index component.</span></span> <span data-ttu-id="7152f-145">Zastąp `<SurveyPrompt>` element z `<Counter>` elementu.</span><span class="sxs-lookup"><span data-stu-id="7152f-145">Replace the `<SurveyPrompt>` element with the `<Counter>` element.</span></span>

   <span data-ttu-id="7152f-146">*Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="7152f-146">*Pages/Index.cshtml*:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Index.cshtml?highlight=7)]

1. <span data-ttu-id="7152f-147">Ponownie skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="7152f-147">Rebuild and run the app.</span></span> <span data-ttu-id="7152f-148">Strona główna ma swój własny licznika.</span><span class="sxs-lookup"><span data-stu-id="7152f-148">The home page has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="7152f-149">Parametry składnika</span><span class="sxs-lookup"><span data-stu-id="7152f-149">Component parameters</span></span>

<span data-ttu-id="7152f-150">Składniki mogą także mieć parametrów.</span><span class="sxs-lookup"><span data-stu-id="7152f-150">Components can also have parameters.</span></span> <span data-ttu-id="7152f-151">Składnik parametry są definiowane za pomocą właściwości niepubliczne klasy składnika ozdobione `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="7152f-151">Component parameters are defined using non-public properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="7152f-152">Używanie atrybutów, aby określić argumenty dla składnika w znacznikach.</span><span class="sxs-lookup"><span data-stu-id="7152f-152">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="7152f-153">Aktualizowanie składnika `@functions` C# kodu:</span><span class="sxs-lookup"><span data-stu-id="7152f-153">Update the component's `@functions` C# code:</span></span>

   * <span data-ttu-id="7152f-154">Dodaj `IncrementAmount` ozdobione właściwość `[Parameter]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="7152f-154">Add a `IncrementAmount` property decorated with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="7152f-155">Zmiana `IncrementCount` metodę `IncrementAmount` podczas zwiększenie wartości `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="7152f-155">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="7152f-156">*Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="7152f-156">*Pages/Counter.cshtml*:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/RazorComponents.App/Pages/Counter.cshtml?highlight=12,16)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. <span data-ttu-id="7152f-157">Określ `IncrementAmount` parametru w części głównej `<Counter>` elementu za pomocą atrybutu.</span><span class="sxs-lookup"><span data-stu-id="7152f-157">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="7152f-158">Ustaw wartość do zwiększenia licznika dziesięciu.</span><span class="sxs-lookup"><span data-stu-id="7152f-158">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="7152f-159">*Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="7152f-159">*Pages/Index.cshtml*:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/RazorComponents.App/Pages/Index.cshtml?highlight=7)]

1. <span data-ttu-id="7152f-160">Ponownie załaduj stronę.</span><span class="sxs-lookup"><span data-stu-id="7152f-160">Reload the page.</span></span> <span data-ttu-id="7152f-161">Licznik strony głównej rośnie przez dziesięć za każdym razem **kliknij mnie** przycisk jest zaznaczony.</span><span class="sxs-lookup"><span data-stu-id="7152f-161">The home page counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="7152f-162">Licznik na *licznika* stronie zwiększa o jeden.</span><span class="sxs-lookup"><span data-stu-id="7152f-162">The counter on the *Counter* page increments by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="7152f-163">Kierowanie do składników</span><span class="sxs-lookup"><span data-stu-id="7152f-163">Route to components</span></span>

<span data-ttu-id="7152f-164">`@page` Dyrektywę w górnej części *Counter.cshtml* pliku Określa, że ten składnik jest routingu punkt końcowy.</span><span class="sxs-lookup"><span data-stu-id="7152f-164">The `@page` directive at the top of the *Counter.cshtml* file specifies that this component is a routing endpoint.</span></span> <span data-ttu-id="7152f-165">Składnik licznika obsługuje żądania wysyłane do `/Counter`.</span><span class="sxs-lookup"><span data-stu-id="7152f-165">The Counter component handles requests sent to `/Counter`.</span></span> <span data-ttu-id="7152f-166">Bez `@page` dyrektywy, składnik nie obsługuje żądania trasowane, ale nadal można używać składnika przez inne składniki.</span><span class="sxs-lookup"><span data-stu-id="7152f-166">Without the `@page` directive, the component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="7152f-167">Wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="7152f-167">Dependency injection</span></span>

<span data-ttu-id="7152f-168">Zarejestrowane w kontenerze usługi app Services są dostępne dla składników za pomocą [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="7152f-168">Services registered in the app's service container are available to components via [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="7152f-169">Wstrzyknięcie usług do składnika za pomocą `@inject` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="7152f-169">Inject services into a component using the `@inject` directive.</span></span>

<span data-ttu-id="7152f-170">Sprawdź dyrektywy składnika FetchData (*Pages/FetchData.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="7152f-170">Examine the directives of the FetchData component (*Pages/FetchData.cshtml*).</span></span> <span data-ttu-id="7152f-171">`@inject` Dyrektywy służy do dodania wystąpienia programu `WeatherForecastService` usługi do składnika:</span><span class="sxs-lookup"><span data-stu-id="7152f-171">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the component:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData1.cshtml?highlight=3)]

<span data-ttu-id="7152f-172">`WeatherForecastService` Usługi jest zarejestrowana jako [pojedyncze](xref:fundamentals/dependency-injection#service-lifetimes), więc jedno wystąpienie usługi jest dostępne w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7152f-172">The `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes), so one instance of the service is available throughout the app.</span></span>

<span data-ttu-id="7152f-173">Składnik FetchData używa usługi wprowadzonego jako `ForecastService`, aby pobrać tablicę `WeatherForecast` obiektów:</span><span class="sxs-lookup"><span data-stu-id="7152f-173">The FetchData component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData2.cshtml?highlight=6)]

<span data-ttu-id="7152f-174">A [ @foreach ](/dotnet/csharp/language-reference/keywords/foreach-in) pętli jest używany do renderowania każde wystąpienie prognozy jako wiersz w tabeli danych o pogodzie:</span><span class="sxs-lookup"><span data-stu-id="7152f-174">A [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData3.cshtml?highlight=11-19)]

## <a name="build-a-todo-list"></a><span data-ttu-id="7152f-175">Tworzenie listy zadań do wykonania</span><span class="sxs-lookup"><span data-stu-id="7152f-175">Build a todo list</span></span>

<span data-ttu-id="7152f-176">Dodaj nową stronę do aplikacji, która implementuje listy zadań do wykonania prostego.</span><span class="sxs-lookup"><span data-stu-id="7152f-176">Add a new page to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="7152f-177">Dodaj pusty plik do *stron* folder o nazwie *Todo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="7152f-177">Add an empty file to the *Pages* folder named *Todo.cshtml*.</span></span>

1. <span data-ttu-id="7152f-178">Podaj początkowe znaczniki dla strony:</span><span class="sxs-lookup"><span data-stu-id="7152f-178">Provide the initial markup for the page:</span></span>

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. <span data-ttu-id="7152f-179">Na stronie zadań do wykonania należy dodać do paska nawigacyjnego.</span><span class="sxs-lookup"><span data-stu-id="7152f-179">Add the Todo page to the navigation bar.</span></span>

   <span data-ttu-id="7152f-180">Składnik NavMenu (*Shared/NavMenu.csthml*) jest używana w układzie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7152f-180">The NavMenu component (*Shared/NavMenu.csthml*) is used in the app's layout.</span></span> <span data-ttu-id="7152f-181">Układy są składniki, które pozwalają uniknąć duplikowania zawartości w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7152f-181">Layouts are components that allow you to avoid duplication of content in the app.</span></span> <span data-ttu-id="7152f-182">Aby uzyskać więcej informacji, zobacz <xref:razor-components/layouts>.</span><span class="sxs-lookup"><span data-stu-id="7152f-182">For more information, see <xref:razor-components/layouts>.</span></span>

   <span data-ttu-id="7152f-183">Dodaj `<NavLink>` strony Todo, dodając następujące znaczniki elementu listy poniżej istniejące elementy listy w *Shared/NavMenu.csthml* pliku:</span><span class="sxs-lookup"><span data-stu-id="7152f-183">Add a `<NavLink>` for the Todo page by adding the following list item markup below the existing list items in the *Shared/NavMenu.csthml* file:</span></span>

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="7152f-184">Ponownie skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="7152f-184">Rebuild and run the app.</span></span> <span data-ttu-id="7152f-185">Odwiedź nową stronę Todo, aby upewnić się, czy działa link do strony zadań do wykonania.</span><span class="sxs-lookup"><span data-stu-id="7152f-185">Visit the new Todo page to confirm that the link to the Todo page works.</span></span>

1. <span data-ttu-id="7152f-186">Dodaj *TodoItem.cs* pliku w folderze głównym projektu na potrzeby przechowywania klasa, która reprezentuje element todo.</span><span class="sxs-lookup"><span data-stu-id="7152f-186">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="7152f-187">Należy użyć następującego C# kod `TodoItem` klasy:</span><span class="sxs-lookup"><span data-stu-id="7152f-187">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/RazorComponents.App/TodoItem.cs)]

1. <span data-ttu-id="7152f-188">Wróć do części Todo (*Todo.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="7152f-188">Return to the Todo component (*Todo.cshtml*):</span></span>

   * <span data-ttu-id="7152f-189">Dodaj pole do zadań do wykonania w `@functions` bloku.</span><span class="sxs-lookup"><span data-stu-id="7152f-189">Add a field for the todos in an `@functions` block.</span></span> <span data-ttu-id="7152f-190">Składnik Todo to pole jest używane do zarządzania stanem listy rzeczy do zrobienia.</span><span class="sxs-lookup"><span data-stu-id="7152f-190">The Todo component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="7152f-191">Dodaj listę nieuporządkowaną znaczników i `foreach` pętli do renderowania każdego elementu todo, jako element listy.</span><span class="sxs-lookup"><span data-stu-id="7152f-191">Add unordered list markup and a `foreach` loop to render each todo item as a list item.</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData4.cshtml?highlight=5-10,12-14)]

1. <span data-ttu-id="7152f-192">Aplikacja wymaga elementów interfejsu użytkownika do dodawania zadań do wykonania do listy.</span><span class="sxs-lookup"><span data-stu-id="7152f-192">The app requires UI elements for adding todos to the list.</span></span> <span data-ttu-id="7152f-193">Dodaj przycisk poniżej listy i tekstowych danych wejściowych:</span><span class="sxs-lookup"><span data-stu-id="7152f-193">Add a text input and a button below the list:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData5.cshtml?highlight=12-13)]

1. <span data-ttu-id="7152f-194">Ponownie skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="7152f-194">Rebuild and run the app.</span></span> <span data-ttu-id="7152f-195">Gdy **dodawania zadań do wykonania** przycisk jest zaznaczony, nic się nie dzieje, ponieważ program obsługi zdarzeń nie jest powiązaną przycisku.</span><span class="sxs-lookup"><span data-stu-id="7152f-195">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="7152f-196">Dodaj `AddTodo` metodę, aby składnik zadań do wykonania i zarejestruj go dla przycisku kliknie, za pomocą `onclick` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="7152f-196">Add an `AddTodo` method to the Todo component and register it for button clicks using the `onclick` attribute:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData6.cshtml?highlight=2,7-10)]

   <span data-ttu-id="7152f-197">`AddTodo` C# Metoda jest wywoływana po wybraniu przycisku.</span><span class="sxs-lookup"><span data-stu-id="7152f-197">The `AddTodo` C# method is called when the button is selected.</span></span>

1. <span data-ttu-id="7152f-198">Aby uzyskać tytuł elementu nowych zadań do wykonania, należy dodać `newTodo` ciąg pola i powiązać wartość wprowadzania przy użyciu tekstu `bind` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="7152f-198">To get the title of the new todo item, add a `newTodo` string field and bind it to the value of the text input using the `bind` attribute:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData7.cshtml?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" bind="@newTodo" />
   ```

1. <span data-ttu-id="7152f-199">Aktualizacja `AddTodo` metody w celu dodania `TodoItem` o określonym tytule do listy.</span><span class="sxs-lookup"><span data-stu-id="7152f-199">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="7152f-200">Wyczyść wartość wprowadzania tekstu, ustawiając `newTodo` na pusty ciąg:</span><span class="sxs-lookup"><span data-stu-id="7152f-200">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData8.cshtml?highlight=19-26)]

1. <span data-ttu-id="7152f-201">Ponownie skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="7152f-201">Rebuild and run the app.</span></span> <span data-ttu-id="7152f-202">Niektórych zadań do wykonania należy dodać do listy rzeczy do zrobienia, aby przetestować nowy kod.</span><span class="sxs-lookup"><span data-stu-id="7152f-202">Add some todos to the todo list to test the new code.</span></span>

1. <span data-ttu-id="7152f-203">Tekst tytułu dla każdego elementu todo zyski, można edytować i pola wyboru mogą pomóc użytkownikowi informacje o ukończonych elementów.</span><span class="sxs-lookup"><span data-stu-id="7152f-203">The title text for each todo item can be made editable and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="7152f-204">Dodaj dane wejściowe pola wyboru dla każdego elementu todo i powiązać jej wartość na `IsDone` właściwości.</span><span class="sxs-lookup"><span data-stu-id="7152f-204">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="7152f-205">Zmiana `@todo.Title` do `<input>` powiązany element `@todo.Title`:</span><span class="sxs-lookup"><span data-stu-id="7152f-205">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData9.cshtml?highlight=5-6)]

1. <span data-ttu-id="7152f-206">Aby sprawdzić, czy te wartości są powiązane, należy zaktualizować `<h1>` nagłówek, aby wyświetlić liczbę zadań do wykonania, które nie są kompletne (`IsDone` jest `false`).</span><span class="sxs-lookup"><span data-stu-id="7152f-206">To verify that these values are bound, update the `<h1>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. <span data-ttu-id="7152f-207">Ukończone składnika Todo (*Todo.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="7152f-207">The completed Todo component (*Todo.cshtml*):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/RazorComponents.App/Pages/Todo.cshtml)]

1. <span data-ttu-id="7152f-208">Ponownie skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="7152f-208">Rebuild and run the app.</span></span> <span data-ttu-id="7152f-209">Dodaj do testowania nowego kodu do wykonania.</span><span class="sxs-lookup"><span data-stu-id="7152f-209">Add todo items to test the new code.</span></span>

## <a name="publish-and-deploy-the-app"></a><span data-ttu-id="7152f-210">Publikowanie i wdrażanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="7152f-210">Publish and deploy the app</span></span>

<span data-ttu-id="7152f-211">Aby opublikować aplikację, zobacz <xref:host-and-deploy/razor-components/index#publish-the-app>.</span><span class="sxs-lookup"><span data-stu-id="7152f-211">To publish the app, see <xref:host-and-deploy/razor-components/index#publish-the-app>.</span></span>
