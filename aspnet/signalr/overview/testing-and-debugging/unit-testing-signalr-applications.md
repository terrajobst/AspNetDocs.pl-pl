---
uid: signalr/overview/testing-and-debugging/unit-testing-signalr-applications
title: Aplikacje sygnalizujące testy jednostkowe | Microsoft Docs
author: bradygaster
description: W tym artykule opisano sposób korzystania z funkcji testów jednostkowych modułu sygnalizującego 2,0.
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: d1983524-e0d5-4ee6-9d87-1f552f7cb964
msc.legacyurl: /signalr/overview/testing-and-debugging/unit-testing-signalr-applications
msc.type: authoredcontent
ms.openlocfilehash: 2cf2e88f141d89971439dc1fc4979849f8dded47
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578674"
---
# <a name="unit-testing-signalr-applications"></a><span data-ttu-id="61b78-103">Testowanie jednostkowe aplikacji SignalR</span><span class="sxs-lookup"><span data-stu-id="61b78-103">Unit Testing SignalR Applications</span></span>

<span data-ttu-id="61b78-104">[Fletcher Patryka](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="61b78-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="61b78-105">W tym artykule opisano korzystanie z funkcji testowania jednostkowego modułu sygnalizującego 2.</span><span class="sxs-lookup"><span data-stu-id="61b78-105">This article describes using the Unit Testing features of SignalR 2.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="61b78-106">Wersje oprogramowania używane w tym temacie</span><span class="sxs-lookup"><span data-stu-id="61b78-106">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="61b78-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="61b78-107">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="61b78-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="61b78-108">.NET 4.5</span></span>
> - <span data-ttu-id="61b78-109">Sygnalizujący wersja 2</span><span class="sxs-lookup"><span data-stu-id="61b78-109">SignalR version 2</span></span>
>
>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="61b78-110">Pytania i Komentarze</span><span class="sxs-lookup"><span data-stu-id="61b78-110">Questions and comments</span></span>
>
> <span data-ttu-id="61b78-111">Prosimy o opinię na temat sposobu, w jaki lubię ten samouczek, i co możemy ulepszyć w komentarzach w dolnej części strony.</span><span class="sxs-lookup"><span data-stu-id="61b78-111">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="61b78-112">Jeśli masz pytania, które nie są bezpośrednio związane z samouczkiem, możesz je ogłosić na [forum ASP.NET](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="61b78-112">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

<a id="unit"></a>
## <a name="unit-testing-signalr-applications"></a><span data-ttu-id="61b78-113">Aplikacje sygnalizujące testy jednostkowe</span><span class="sxs-lookup"><span data-stu-id="61b78-113">Unit testing SignalR applications</span></span>

<span data-ttu-id="61b78-114">Aby utworzyć testy jednostkowe dla aplikacji sygnalizującej, można użyć funkcji testów jednostkowych w sygnale 2.</span><span class="sxs-lookup"><span data-stu-id="61b78-114">You can use the unit test features in SignalR 2 to create unit tests for your SignalR application.</span></span> <span data-ttu-id="61b78-115">Sygnał 2 zawiera interfejs [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) , który może służyć do tworzenia obiektu makiety do symulowania metod centrów do testowania.</span><span class="sxs-lookup"><span data-stu-id="61b78-115">SignalR 2 includes the [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) interface, which can be used to create a mock object to simulate your hub methods for testing.</span></span>

<span data-ttu-id="61b78-116">W tej sekcji dodasz testy jednostkowe dla aplikacji utworzonej w [samouczku wprowadzenie](../getting-started/tutorial-getting-started-with-signalr.md) przy użyciu [XUnit.NET](https://github.com/xunit/xunit) i [MOQ](https://github.com/Moq/moq4).</span><span class="sxs-lookup"><span data-stu-id="61b78-116">In this section, you'll add unit tests for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using [XUnit.net](https://github.com/xunit/xunit) and [Moq](https://github.com/Moq/moq4).</span></span>

<span data-ttu-id="61b78-117">XUnit.net zostanie użyta do kontroli testu; MOQ zostanie użyta do utworzenia obiektu [makiety](http://en.wikipedia.org/wiki/Mock_object) do testowania.</span><span class="sxs-lookup"><span data-stu-id="61b78-117">XUnit.net will be used to control the test; Moq will be used to create a [mock](http://en.wikipedia.org/wiki/Mock_object) object for testing.</span></span> <span data-ttu-id="61b78-118">W razie potrzeby można użyć innych struktur do imitacji; [NSubstitute](http://nsubstitute.github.io/) jest również dobrym wyborem.</span><span class="sxs-lookup"><span data-stu-id="61b78-118">Other mocking frameworks can be used if desired; [NSubstitute](http://nsubstitute.github.io/) is also a good choice.</span></span> <span data-ttu-id="61b78-119">W tym samouczku pokazano, jak skonfigurować obiekt makiety na dwa sposoby: najpierw przy użyciu obiektu `dynamic` (wprowadzony w .NET Framework 4), a drugi przy użyciu interfejsu.</span><span class="sxs-lookup"><span data-stu-id="61b78-119">This tutorial demonstrates how to set up the mock object in two ways: First, using a `dynamic` object (introduced in .NET Framework 4), and second, using an interface.</span></span>

### <a name="contents"></a><span data-ttu-id="61b78-120">Spis treści</span><span class="sxs-lookup"><span data-stu-id="61b78-120">Contents</span></span>

<span data-ttu-id="61b78-121">Ten samouczek zawiera następujące sekcje.</span><span class="sxs-lookup"><span data-stu-id="61b78-121">This tutorial contains the following sections.</span></span>

- [<span data-ttu-id="61b78-122">Testy jednostkowe z dynamiczną</span><span class="sxs-lookup"><span data-stu-id="61b78-122">Unit testing with Dynamic</span></span>](#dynamic)
- [<span data-ttu-id="61b78-123">Testy jednostkowe według typu</span><span class="sxs-lookup"><span data-stu-id="61b78-123">Unit testing by type</span></span>](#type)

<a id="dynamic"></a>
### <a name="unit-testing-with-dynamic"></a><span data-ttu-id="61b78-124">Testy jednostkowe z dynamiczną</span><span class="sxs-lookup"><span data-stu-id="61b78-124">Unit testing with Dynamic</span></span>

<span data-ttu-id="61b78-125">W tej sekcji dodasz test jednostkowy dla aplikacji utworzonej w [samouczku wprowadzenie](../getting-started/tutorial-getting-started-with-signalr.md) przy użyciu obiektu dynamicznego.</span><span class="sxs-lookup"><span data-stu-id="61b78-125">In this section, you'll add a unit test for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using a dynamic object.</span></span>

1. <span data-ttu-id="61b78-126">Zainstaluj [rozszerzenie XUnit Runner](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) dla Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="61b78-126">Install the [XUnit Runner extension](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) for Visual Studio 2013.</span></span>
2. <span data-ttu-id="61b78-127">Ukończ [samouczek wprowadzenie](../getting-started/tutorial-getting-started-with-signalr.md)lub Pobierz ukończoną aplikację z [galerii kodu MSDN](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span><span class="sxs-lookup"><span data-stu-id="61b78-127">Either complete the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md), or download the completed application from [MSDN Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span></span>
3. <span data-ttu-id="61b78-128">Jeśli używasz wersji pobierania Wprowadzenie aplikacji, Otwórz **konsolę Menedżera pakietów** i kliknij przycisk **Przywróć** , aby dodać pakiet sygnalizujący do projektu.</span><span class="sxs-lookup"><span data-stu-id="61b78-128">If you are using the download version of the Getting Started application, open **Package Manager Console** and click **Restore** to add the SignalR package to the project.</span></span>

    ![Przywróć pakiety](unit-testing-signalr-applications/_static/image1.png)
4. <span data-ttu-id="61b78-130">Dodaj projekt do rozwiązania dla testu jednostkowego.</span><span class="sxs-lookup"><span data-stu-id="61b78-130">Add a project to the solution for the unit test.</span></span> <span data-ttu-id="61b78-131">Kliknij prawym przyciskiem myszy rozwiązanie w **Eksplorator rozwiązań** i wybierz polecenie **Dodaj**, **Nowy projekt..** .. W **C#** węźle wybierz węzeł **systemu Windows** .</span><span class="sxs-lookup"><span data-stu-id="61b78-131">Right-click your solution in **Solution Explorer** and select **Add**, **New Project...**. Under the **C#** node, select the **Windows** node.</span></span> <span data-ttu-id="61b78-132">Wybierz **bibliotekę klas**.</span><span class="sxs-lookup"><span data-stu-id="61b78-132">Select **Class Library**.</span></span> <span data-ttu-id="61b78-133">Nazwij nowy projekt **TestLibrary** i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="61b78-133">Name the new project **TestLibrary** and click **OK**.</span></span>

    ![Utwórz bibliotekę testową](unit-testing-signalr-applications/_static/image2.png)
5. <span data-ttu-id="61b78-135">Dodaj odwołanie w projekcie biblioteki testowej do projektu SignalRChat.</span><span class="sxs-lookup"><span data-stu-id="61b78-135">Add a reference in the test library project to the SignalRChat project.</span></span> <span data-ttu-id="61b78-136">Kliknij prawym przyciskiem myszy projekt **TestLibrary** i wybierz polecenie **Dodaj**, **odwołanie...** . Wybierz węzeł **projekty** w węźle **rozwiązanie** i sprawdź **SignalRChat**.</span><span class="sxs-lookup"><span data-stu-id="61b78-136">Right-click the **TestLibrary** project and select **Add**, **Reference...**. Select the **Projects** node under the **Solution** node, and check **SignalRChat**.</span></span> <span data-ttu-id="61b78-137">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="61b78-137">Click **OK**.</span></span>

    ![Dodaj odwołanie do projektu](unit-testing-signalr-applications/_static/image3.png)
6. <span data-ttu-id="61b78-139">Dodaj pakiety sygnalizujące, MOQ i XUnit do projektu **TestLibrary** .</span><span class="sxs-lookup"><span data-stu-id="61b78-139">Add the SignalR, Moq, and XUnit packages to the **TestLibrary** project.</span></span> <span data-ttu-id="61b78-140">W **konsoli Menedżera pakietów**Ustaw listę rozwijaną **domyślny projekt** na **TestLibrary**.</span><span class="sxs-lookup"><span data-stu-id="61b78-140">In the **Package Manager Console**, set the **Default Project** dropdown to **TestLibrary**.</span></span> <span data-ttu-id="61b78-141">Uruchom następujące polecenia w oknie konsoli:</span><span class="sxs-lookup"><span data-stu-id="61b78-141">Run the following commands in the console window:</span></span>

   - `Install-Package Microsoft.AspNet.SignalR`
   - `Install-Package Moq`
   - `Install-Package XUnit`

     ![Zainstaluj pakiety](unit-testing-signalr-applications/_static/image4.png)
7. <span data-ttu-id="61b78-143">Utwórz plik testowy.</span><span class="sxs-lookup"><span data-stu-id="61b78-143">Create the test file.</span></span> <span data-ttu-id="61b78-144">Kliknij prawym przyciskiem myszy projekt **TestLibrary** , a następnie kliknij przycisk **Dodaj...** , **Klasa**.</span><span class="sxs-lookup"><span data-stu-id="61b78-144">Right-click the **TestLibrary** project and click **Add...**, **Class**.</span></span> <span data-ttu-id="61b78-145">Nadaj nowej klasie nazwę **tests.cs**.</span><span class="sxs-lookup"><span data-stu-id="61b78-145">Name the new class **Tests.cs**.</span></span>
8. <span data-ttu-id="61b78-146">Zastąp zawartość Tests.cs następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="61b78-146">Replace the contents of Tests.cs with the following code.</span></span>

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample1.cs)]

    <span data-ttu-id="61b78-147">W powyższym kodzie klient testowy jest tworzony przy użyciu obiektu `Mock` z biblioteki [MOQ](https://github.com/Moq/moq4) , typu [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (w sygnalizacji 2,1, przypisz `dynamic` parametru typu). Interfejs `IHubCallerConnectionContext` jest obiektem proxy, za pomocą którego są wywoływane metody na kliencie.</span><span class="sxs-lookup"><span data-stu-id="61b78-147">In the code above, a test client is created using the `Mock` object from the [Moq](https://github.com/Moq/moq4) library, of type [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in SignalR 2.1, assign `dynamic` for the type parameter.) The `IHubCallerConnectionContext` interface is the proxy object with which you invoke methods on the client.</span></span> <span data-ttu-id="61b78-148">Funkcja `broadcastMessage` jest następnie definiowana dla klienta makiety, dzięki czemu może być wywoływana przez klasę `ChatHub`.</span><span class="sxs-lookup"><span data-stu-id="61b78-148">The `broadcastMessage` function is then defined for the mock client so that it can be called by the `ChatHub` class.</span></span> <span data-ttu-id="61b78-149">Aparat testowy następnie wywołuje metodę `Send` klasy `ChatHub`, która z kolei wywołuje funkcję `broadcastMessage` zamakietowania.</span><span class="sxs-lookup"><span data-stu-id="61b78-149">The test engine then calls the `Send` method of the `ChatHub` class, which in turn calls the mocked `broadcastMessage` function.</span></span>
9. <span data-ttu-id="61b78-150">Skompiluj rozwiązanie, naciskając klawisz **F6**.</span><span class="sxs-lookup"><span data-stu-id="61b78-150">Build the solution by pressing **F6**.</span></span>
10. <span data-ttu-id="61b78-151">Uruchom test jednostkowy.</span><span class="sxs-lookup"><span data-stu-id="61b78-151">Run the unit test.</span></span> <span data-ttu-id="61b78-152">W programie Visual Studio wybierz kolejno opcje **test**, **Windows**i **Eksplorator testów**.</span><span class="sxs-lookup"><span data-stu-id="61b78-152">In Visual Studio, select **Test**, **Windows**, **Test Explorer**.</span></span> <span data-ttu-id="61b78-153">W oknie Eksplorator testów kliknij prawym przyciskiem myszy pozycję **HubsAreMockableViaDynamic** i wybierz polecenie **Uruchom wybrane testy**.</span><span class="sxs-lookup"><span data-stu-id="61b78-153">In the Test Explorer window, right-click **HubsAreMockableViaDynamic** and select **Run Selected Tests**.</span></span>

    ![Eksplorator testów](unit-testing-signalr-applications/_static/image5.png)
11. <span data-ttu-id="61b78-155">Sprawdź, czy test zakończono testy, sprawdzając dolne okienko w oknie Eksplorator testów.</span><span class="sxs-lookup"><span data-stu-id="61b78-155">Verify that the test passed by checking the lower pane in the Test Explorer window.</span></span> <span data-ttu-id="61b78-156">Zostanie wyświetlone okno pokazujące, że test zakończono.</span><span class="sxs-lookup"><span data-stu-id="61b78-156">The window will show that the test passed.</span></span>

    ![Test zakończony niepowodzenie](unit-testing-signalr-applications/_static/image6.png)

<a id="type"></a>
### <a name="unit-testing-by-type"></a><span data-ttu-id="61b78-158">Testy jednostkowe według typu</span><span class="sxs-lookup"><span data-stu-id="61b78-158">Unit testing by type</span></span>

<span data-ttu-id="61b78-159">W tej sekcji dodasz test dla aplikacji utworzonej w [samouczku wprowadzenie](../getting-started/tutorial-getting-started-with-signalr.md) przy użyciu interfejsu, który zawiera metodę do przetestowania.</span><span class="sxs-lookup"><span data-stu-id="61b78-159">In this section, you'll add a test for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using an interface that contains the method to be tested.</span></span>

1. <span data-ttu-id="61b78-160">Wykonaj kroki 1-7 w ramach [testów jednostkowych za pomocą](#dynamic) powyższego samouczka dynamicznego.</span><span class="sxs-lookup"><span data-stu-id="61b78-160">Complete steps 1-7 in the [Unit testing with Dynamic](#dynamic) tutorial above.</span></span>
2. <span data-ttu-id="61b78-161">Zastąp zawartość Tests.cs następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="61b78-161">Replace the contents of Tests.cs with the following code.</span></span>

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample2.cs)]

    <span data-ttu-id="61b78-162">W powyższym kodzie tworzony jest interfejs definiujący sygnaturę metody `broadcastMessage`, dla której aparat testowy utworzy klienta programu.</span><span class="sxs-lookup"><span data-stu-id="61b78-162">In the code above, an interface is created defining the signature of the `broadcastMessage` method for which the test engine will create a mock client.</span></span> <span data-ttu-id="61b78-163">Klient, który jest następnie tworzony przy użyciu obiektu `Mock`, typu [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (w sygnalizującer 2,1, przypisz `dynamic` parametru typu). Interfejs `IHubCallerConnectionContext` jest obiektem proxy, za pomocą którego są wywoływane metody na kliencie.</span><span class="sxs-lookup"><span data-stu-id="61b78-163">A mock client is then created using the `Mock` object, of type [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in SignalR 2.1, assign `dynamic` for the type parameter.) The `IHubCallerConnectionContext` interface is the proxy object with which you invoke methods on the client.</span></span>

    <span data-ttu-id="61b78-164">Następnie test tworzy wystąpienie `ChatHub`, a następnie tworzy wersję makiety metody `broadcastMessage`, która z kolei jest wywoływana przez wywołanie metody `Send` w centrum.</span><span class="sxs-lookup"><span data-stu-id="61b78-164">The test then creates an instance of `ChatHub`, and then creates a mock version of the `broadcastMessage` method, which in turn is invoked by calling the `Send` method on the hub.</span></span>
3. <span data-ttu-id="61b78-165">Skompiluj rozwiązanie, naciskając klawisz **F6**.</span><span class="sxs-lookup"><span data-stu-id="61b78-165">Build the solution by pressing **F6**.</span></span>
4. <span data-ttu-id="61b78-166">Uruchom test jednostkowy.</span><span class="sxs-lookup"><span data-stu-id="61b78-166">Run the unit test.</span></span> <span data-ttu-id="61b78-167">W programie Visual Studio wybierz kolejno opcje **test**, **Windows**i **Eksplorator testów**.</span><span class="sxs-lookup"><span data-stu-id="61b78-167">In Visual Studio, select **Test**, **Windows**, **Test Explorer**.</span></span> <span data-ttu-id="61b78-168">W oknie Eksplorator testów kliknij prawym przyciskiem myszy pozycję **HubsAreMockableViaDynamic** i wybierz polecenie **Uruchom wybrane testy**.</span><span class="sxs-lookup"><span data-stu-id="61b78-168">In the Test Explorer window, right-click **HubsAreMockableViaDynamic** and select **Run Selected Tests**.</span></span>

    ![Eksplorator testów](unit-testing-signalr-applications/_static/image7.png)
5. <span data-ttu-id="61b78-170">Sprawdź, czy test zakończono testy, sprawdzając dolne okienko w oknie Eksplorator testów.</span><span class="sxs-lookup"><span data-stu-id="61b78-170">Verify that the test passed by checking the lower pane in the Test Explorer window.</span></span> <span data-ttu-id="61b78-171">Zostanie wyświetlone okno pokazujące, że test zakończono.</span><span class="sxs-lookup"><span data-stu-id="61b78-171">The window will show that the test passed.</span></span>

    ![Test zakończony niepowodzenie](unit-testing-signalr-applications/_static/image8.png)
