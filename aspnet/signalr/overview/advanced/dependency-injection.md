---
uid: signalr/overview/advanced/dependency-injection
title: Iniekcja zależności w sygnale | Microsoft Docs
author: bradygaster
description: Wersje oprogramowania używane w tym temacie Visual Studio 2013 programu .NET 4,5 sygnalizującego w wersji 2 poprzednie wersje tego tematu, aby uzyskać informacje o wcześniejszych wersjach programu...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: a14121ae-02cf-4024-8af0-9dd0dc810690
msc.legacyurl: /signalr/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 52978b10b6c131ac8eff4535216cc60b43fdf3de
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78537332"
---
# <a name="dependency-injection-in-signalr"></a><span data-ttu-id="8552b-103">Wstrzykiwanie zależności w usłudze SignalR</span><span class="sxs-lookup"><span data-stu-id="8552b-103">Dependency Injection in SignalR</span></span>

<span data-ttu-id="8552b-104">według [Jan Wasson](https://github.com/MikeWasson), [Patryk Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="8552b-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="8552b-105">Wersje oprogramowania używane w tym temacie</span><span class="sxs-lookup"><span data-stu-id="8552b-105">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="8552b-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="8552b-106">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="8552b-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="8552b-107">.NET 4.5</span></span>
> - <span data-ttu-id="8552b-108">Sygnalizujący wersja 2</span><span class="sxs-lookup"><span data-stu-id="8552b-108">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="8552b-109">Poprzednie wersje tego tematu</span><span class="sxs-lookup"><span data-stu-id="8552b-109">Previous versions of this topic</span></span>
>
> <span data-ttu-id="8552b-110">Aby uzyskać informacje o wcześniejszych wersjach programu sygnalizującego, zobacz sekcję [sygnalizujące starsze wersje](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="8552b-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="8552b-111">Pytania i Komentarze</span><span class="sxs-lookup"><span data-stu-id="8552b-111">Questions and comments</span></span>
>
> <span data-ttu-id="8552b-112">Prosimy o opinię na temat sposobu, w jaki lubię ten samouczek, i co możemy ulepszyć w komentarzach w dolnej części strony.</span><span class="sxs-lookup"><span data-stu-id="8552b-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="8552b-113">Jeśli masz pytania, które nie są bezpośrednio związane z samouczkiem, możesz je ogłosić na [forum ASP.NET](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="8552b-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

<span data-ttu-id="8552b-114">Iniekcja zależności polega na usunięciu sztywnych zależności między obiektami, ułatwiając zastępowanie zależności obiektu, w przypadku testowania (przy użyciu obiektów makiety) lub zmiany zachowania w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="8552b-114">Dependency injection is a way to remove hard-coded dependencies between objects, making it easier to replace an object's dependencies, either for testing (using mock objects) or to change run-time behavior.</span></span> <span data-ttu-id="8552b-115">Ten samouczek pokazuje, jak przeprowadzić iniekcję zależności w centrach sygnałów.</span><span class="sxs-lookup"><span data-stu-id="8552b-115">This tutorial shows how to perform dependency injection on SignalR hubs.</span></span> <span data-ttu-id="8552b-116">Przedstawiono w nim również, jak używać kontenerów IoC z sygnalizacyjnym.</span><span class="sxs-lookup"><span data-stu-id="8552b-116">It also shows how to use IoC containers with SignalR.</span></span> <span data-ttu-id="8552b-117">Kontener IoC to ogólna struktura iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="8552b-117">An IoC container is a general framework for dependency injection.</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="8552b-118">Co to jest iniekcja zależności?</span><span class="sxs-lookup"><span data-stu-id="8552b-118">What is Dependency Injection?</span></span>

<span data-ttu-id="8552b-119">Pomiń tę sekcję, jeśli znasz już funkcję iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="8552b-119">Skip this section if you are already familiar with dependency injection.</span></span>

<span data-ttu-id="8552b-120">*Iniekcja zależności* (di) jest wzorcem, w którym obiekty nie są odpowiedzialne za tworzenie własnych zależności.</span><span class="sxs-lookup"><span data-stu-id="8552b-120">*Dependency injection* (DI) is a pattern where objects are not responsible for creating their own dependencies.</span></span> <span data-ttu-id="8552b-121">Oto prosty przykład do motywu DI.</span><span class="sxs-lookup"><span data-stu-id="8552b-121">Here is a simple example to motivate DI.</span></span> <span data-ttu-id="8552b-122">Załóżmy, że masz obiekt, który musi rejestrować komunikaty.</span><span class="sxs-lookup"><span data-stu-id="8552b-122">Suppose you have an object that needs to log messages.</span></span> <span data-ttu-id="8552b-123">Można zdefiniować interfejs rejestrowania:</span><span class="sxs-lookup"><span data-stu-id="8552b-123">You might define a logging interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="8552b-124">W obiekcie można utworzyć `ILogger` do rejestrowania komunikatów:</span><span class="sxs-lookup"><span data-stu-id="8552b-124">In your object, you can create an `ILogger` to log messages:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="8552b-125">To działa, ale nie jest najlepszym projektem.</span><span class="sxs-lookup"><span data-stu-id="8552b-125">This works, but it's not the best design.</span></span> <span data-ttu-id="8552b-126">Jeśli chcesz zastąpić `FileLogger` inną implementacją `ILogger`, musisz zmodyfikować `SomeComponent`.</span><span class="sxs-lookup"><span data-stu-id="8552b-126">If you want to replace `FileLogger` with another `ILogger` implementation, you will have to modify `SomeComponent`.</span></span> <span data-ttu-id="8552b-127">Załóżmy, że wiele innych obiektów używa `FileLogger`, należy zmienić wszystkie z nich.</span><span class="sxs-lookup"><span data-stu-id="8552b-127">Supposing that a lot of other objects use `FileLogger`, you will need to change all of them.</span></span> <span data-ttu-id="8552b-128">Lub jeśli zdecydujesz się wprowadzić `FileLogger` pojedynczej, musisz również wprowadzić zmiany w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8552b-128">Or if you decide to make `FileLogger` a singleton, you'll also need to make changes throughout the application.</span></span>

<span data-ttu-id="8552b-129">Lepszym rozwiązaniem jest "wstrzyknięcie" `ILogger` do obiektu — na przykład przy użyciu argumentu konstruktora:</span><span class="sxs-lookup"><span data-stu-id="8552b-129">A better approach is to "inject" an `ILogger` into the object—for example, by using a constructor argument:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="8552b-130">Teraz obiekt nie odpowiada za wybór `ILogger` do użycia.</span><span class="sxs-lookup"><span data-stu-id="8552b-130">Now the object is not responsible for selecting which `ILogger` to use.</span></span> <span data-ttu-id="8552b-131">Można zmienić implementacje `ILogger` bez zmiany obiektów, które są od niego zależne.</span><span class="sxs-lookup"><span data-stu-id="8552b-131">You can switch `ILogger` implementations without changing the objects that depend on it.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="8552b-132">Ten wzorzec jest nazywany [iniekcją konstruktora](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="8552b-132">This pattern is called [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="8552b-133">Inny wzorzec to iniekcja setter, w której ustawia się zależność za pomocą metody ustawiającej lub właściwości.</span><span class="sxs-lookup"><span data-stu-id="8552b-133">Another pattern is setter injection, where you set the dependency through a setter method or property.</span></span>

## <a name="simple-dependency-injection-in-signalr"></a><span data-ttu-id="8552b-134">Proste iniekcja zależności w sygnale</span><span class="sxs-lookup"><span data-stu-id="8552b-134">Simple Dependency Injection in SignalR</span></span>

<span data-ttu-id="8552b-135">Rozważmy aplikację Chat z samouczka [wprowadzenie za pomocą programu sygnalizującego](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="8552b-135">Consider the Chat application from the tutorial [Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="8552b-136">Oto Klasa centrum z tej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="8552b-136">Here is the hub class from that application:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="8552b-137">Załóżmy, że chcesz przechowywać komunikaty czatu na serwerze przed ich wysłaniem.</span><span class="sxs-lookup"><span data-stu-id="8552b-137">Suppose that you want to store chat messages on the server before sending them.</span></span> <span data-ttu-id="8552b-138">Można zdefiniować interfejs, który jest abstrakcyjny dla tej funkcji, i użyć funkcji DI, aby wstrzyknąć interfejs do klasy `ChatHub`.</span><span class="sxs-lookup"><span data-stu-id="8552b-138">You might define an interface that abstracts this functionality, and use DI to inject the interface into the `ChatHub` class.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="8552b-139">Jedyny problem polega na tym, że aplikacja sygnalizująca nie tworzy bezpośrednio centrów. Tworzy je dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="8552b-139">The only problem is that a SignalR application does not directly create hubs; SignalR creates them for you.</span></span> <span data-ttu-id="8552b-140">Domyślnie program sygnalizujący oczekuje, że Klasa centrum ma Konstruktor bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="8552b-140">By default, SignalR expects a hub class to have a parameterless constructor.</span></span> <span data-ttu-id="8552b-141">Można jednak łatwo zarejestrować funkcję do tworzenia wystąpień centrów i użyć tej funkcji do wykonania DI.</span><span class="sxs-lookup"><span data-stu-id="8552b-141">However, you can easily register a function to create hub instances, and use this function to perform DI.</span></span> <span data-ttu-id="8552b-142">Zarejestruj funkcję przez wywołanie **GlobalHost. DependencyResolver. Register**.</span><span class="sxs-lookup"><span data-stu-id="8552b-142">Register the function by calling **GlobalHost.DependencyResolver.Register**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

<span data-ttu-id="8552b-143">Teraz sygnalizujący wywoła tę funkcję anonimową za każdym razem, gdy musi utworzyć wystąpienie `ChatHub`.</span><span class="sxs-lookup"><span data-stu-id="8552b-143">Now SignalR will invoke this anonymous function whenever it needs to create a `ChatHub` instance.</span></span>

## <a name="ioc-containers"></a><span data-ttu-id="8552b-144">Kontenery IoC</span><span class="sxs-lookup"><span data-stu-id="8552b-144">IoC Containers</span></span>

<span data-ttu-id="8552b-145">Poprzedni kod jest bardzo drobny w przypadku prostych przypadków.</span><span class="sxs-lookup"><span data-stu-id="8552b-145">The previous code is fine for simple cases.</span></span> <span data-ttu-id="8552b-146">Jednak nadal musiałeś napisać:</span><span class="sxs-lookup"><span data-stu-id="8552b-146">But you still had to write this:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

<span data-ttu-id="8552b-147">W złożonej aplikacji z wieloma zależnościami może być konieczne napisanie wielu tych kodów "okablowania".</span><span class="sxs-lookup"><span data-stu-id="8552b-147">In a complex application with many dependencies, you might need to write a lot of this "wiring" code.</span></span> <span data-ttu-id="8552b-148">Ten kod może być trudny do utrzymania, szczególnie jeśli zależności są zagnieżdżone.</span><span class="sxs-lookup"><span data-stu-id="8552b-148">This code can be hard to maintain, especially if dependencies are nested.</span></span> <span data-ttu-id="8552b-149">Test jednostkowy jest również trudne.</span><span class="sxs-lookup"><span data-stu-id="8552b-149">It is also hard to unit test.</span></span>

<span data-ttu-id="8552b-150">Jednym z rozwiązań jest użycie kontenera IoC.</span><span class="sxs-lookup"><span data-stu-id="8552b-150">One solution is to use an IoC container.</span></span> <span data-ttu-id="8552b-151">Kontener IoC to składnik oprogramowania, który jest odpowiedzialny za zarządzanie zależnościami. Możesz zarejestrować typy w kontenerze, a następnie użyć kontenera do tworzenia obiektów.</span><span class="sxs-lookup"><span data-stu-id="8552b-151">An IoC container is a software component that is responsible for managing dependencies.You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="8552b-152">Kontener automatycznie określa relacje zależności.</span><span class="sxs-lookup"><span data-stu-id="8552b-152">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="8552b-153">Wiele kontenerów IoC umożliwia również Sterowanie elementami, takimi jak okres istnienia obiektu i zakres.</span><span class="sxs-lookup"><span data-stu-id="8552b-153">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="8552b-154">"IoC" oznacza "Inversion of Control", który jest ogólnym wzorcem, w którym struktura wywołuje kod aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8552b-154">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="8552b-155">Kontener IoC konstruuje obiekty, co oznacza, że jest to normalny przepływ sterowania.</span><span class="sxs-lookup"><span data-stu-id="8552b-155">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>

## <a name="using-ioc-containers-in-signalr"></a><span data-ttu-id="8552b-156">Używanie kontenerów IoC w sygnalizacji</span><span class="sxs-lookup"><span data-stu-id="8552b-156">Using IoC Containers in SignalR</span></span>

<span data-ttu-id="8552b-157">Aplikacja Chat jest prawdopodobnie zbyt prosta, aby można było skorzystać z kontenera IoC.</span><span class="sxs-lookup"><span data-stu-id="8552b-157">The Chat application is probably too simple to benefit from an IoC container.</span></span> <span data-ttu-id="8552b-158">Zamiast tego Przyjrzyjmy się przykładowi [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) .</span><span class="sxs-lookup"><span data-stu-id="8552b-158">Instead, let's look at the [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) sample.</span></span>

<span data-ttu-id="8552b-159">Przykład StockTicker definiuje dwie klasy główne:</span><span class="sxs-lookup"><span data-stu-id="8552b-159">The StockTicker sample defines two main classes:</span></span>

- <span data-ttu-id="8552b-160">`StockTickerHub`: Klasa Hub, która zarządza połączeniami klientów.</span><span class="sxs-lookup"><span data-stu-id="8552b-160">`StockTickerHub`: The hub class, which manages client connections.</span></span>
- <span data-ttu-id="8552b-161">`StockTicker`: pojedynczy, który przechowuje ceny giełdowe, i okresowo aktualizuje je.</span><span class="sxs-lookup"><span data-stu-id="8552b-161">`StockTicker`: A singleton that holds stock prices and periodically updates them.</span></span>

<span data-ttu-id="8552b-162">`StockTickerHub` przechowuje odwołanie do `StockTicker` pojedynczej, podczas gdy `StockTicker` przechowuje odwołanie do **IHubConnectionContext** dla `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="8552b-162">`StockTickerHub` holds a reference to the `StockTicker` singleton, while `StockTicker` holds a reference to the **IHubConnectionContext** for the `StockTickerHub`.</span></span> <span data-ttu-id="8552b-163">Używa tego interfejsu do komunikowania się z wystąpieniami `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="8552b-163">It uses this interface to communicate with `StockTickerHub` instances.</span></span> <span data-ttu-id="8552b-164">(Aby uzyskać więcej informacji, zobacz [Server Broadcast with ASP.NET signaler](../getting-started/tutorial-server-broadcast-with-signalr.md)).</span><span class="sxs-lookup"><span data-stu-id="8552b-164">(For more information, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).)</span></span>

<span data-ttu-id="8552b-165">Możemy użyć kontenera IoC, aby porządkowaniem te zależności jako bity.</span><span class="sxs-lookup"><span data-stu-id="8552b-165">We can use an IoC container to untangle these dependencies a bit.</span></span> <span data-ttu-id="8552b-166">Najpierw uprośmy klasy `StockTickerHub` i `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="8552b-166">First, let's simplify the `StockTickerHub` and `StockTicker` classes.</span></span> <span data-ttu-id="8552b-167">W poniższym kodzie mam komentarz do niepotrzebnych elementów.</span><span class="sxs-lookup"><span data-stu-id="8552b-167">In the following code, I've commented out the parts that we don't need.</span></span>

<span data-ttu-id="8552b-168">Usuń Konstruktor bez parametrów z `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="8552b-168">Remove the parameterless constructor from `StockTickerHub`.</span></span> <span data-ttu-id="8552b-169">Zamiast tego zawsze będziemy używać narzędzia DI do utworzenia centrum.</span><span class="sxs-lookup"><span data-stu-id="8552b-169">Instead, we will always use DI to create the hub.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

<span data-ttu-id="8552b-170">W przypadku StockTicker Usuń pojedyncze wystąpienie.</span><span class="sxs-lookup"><span data-stu-id="8552b-170">For StockTicker, remove the singleton instance.</span></span> <span data-ttu-id="8552b-171">Później będziemy używać kontenera IoC do kontrolowania okresu istnienia StockTicker.</span><span class="sxs-lookup"><span data-stu-id="8552b-171">Later, we'll use the IoC container to control the StockTicker lifetime.</span></span> <span data-ttu-id="8552b-172">Ponadto ustaw konstruktora jako publiczny.</span><span class="sxs-lookup"><span data-stu-id="8552b-172">Also, make the constructor public.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

<span data-ttu-id="8552b-173">Następnie możemy zakodować kod przez utworzenie interfejsu dla `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="8552b-173">Next, we can refactor the code by creating an interface for `StockTicker`.</span></span> <span data-ttu-id="8552b-174">Użyjemy tego interfejsu, aby rozdzielić `StockTickerHub` z klasy `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="8552b-174">We'll use this interface to decouple the `StockTickerHub` from the `StockTicker` class.</span></span>

<span data-ttu-id="8552b-175">Program Visual Studio ułatwia ten rodzaj refaktoryzacji.</span><span class="sxs-lookup"><span data-stu-id="8552b-175">Visual Studio makes this kind of refactoring easy.</span></span> <span data-ttu-id="8552b-176">Otwórz plik StockTicker.cs, kliknij prawym przyciskiem myszy deklarację klasy `StockTicker` i wybierz pozycję **Refaktoryzacja** ... **Wyodrębnij interfejs**.</span><span class="sxs-lookup"><span data-stu-id="8552b-176">Open the file StockTicker.cs, right-click on the `StockTicker` class declaration, and select **Refactor** ... **Extract Interface**.</span></span>

![](dependency-injection/_static/image1.png)

<span data-ttu-id="8552b-177">W oknie dialogowym **wyodrębnianie interfejsu** kliknij pozycję **Zaznacz wszystko**.</span><span class="sxs-lookup"><span data-stu-id="8552b-177">In the **Extract Interface** dialog, click **Select All**.</span></span> <span data-ttu-id="8552b-178">Pozostaw inne wartości domyślne.</span><span class="sxs-lookup"><span data-stu-id="8552b-178">Leave the other defaults.</span></span> <span data-ttu-id="8552b-179">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="8552b-179">Click **OK**.</span></span>

![](dependency-injection/_static/image2.png)

<span data-ttu-id="8552b-180">Program Visual Studio tworzy nowy interfejs o nazwie `IStockTicker`, a także zmienia `StockTicker` na pochodny od `IStockTicker`.</span><span class="sxs-lookup"><span data-stu-id="8552b-180">Visual Studio creates a new interface named `IStockTicker`, and also changes `StockTicker` to derive from `IStockTicker`.</span></span>

<span data-ttu-id="8552b-181">Otwórz plik IStockTicker.cs i Zmień interfejs na **publiczny**.</span><span class="sxs-lookup"><span data-stu-id="8552b-181">Open the file IStockTicker.cs and change the interface to **public**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

<span data-ttu-id="8552b-182">W klasie `StockTickerHub` Zmień dwa wystąpienia `StockTicker` na `IStockTicker`:</span><span class="sxs-lookup"><span data-stu-id="8552b-182">In the `StockTickerHub` class, change the two instances of `StockTicker` to `IStockTicker`:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

<span data-ttu-id="8552b-183">Tworzenie interfejsu `IStockTicker` nie jest absolutnie konieczne, ale chcę pokazać, jak DI może pomóc w zmniejszeniu sprzęgania między składnikami w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8552b-183">Creating an `IStockTicker` interface isn't strictly necessary, but I wanted to show how DI can help to reduce coupling between components in your application.</span></span>

## <a name="add-the-ninject-library"></a><span data-ttu-id="8552b-184">Dodaj bibliotekę Ninject</span><span class="sxs-lookup"><span data-stu-id="8552b-184">Add the Ninject Library</span></span>

<span data-ttu-id="8552b-185">Istnieje wiele kontenerów IoC "open source" dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="8552b-185">There are many open-source IoC containers for .NET.</span></span> <span data-ttu-id="8552b-186">W tym samouczku użyjemy [Ninject](http://www.ninject.org/).</span><span class="sxs-lookup"><span data-stu-id="8552b-186">For this tutorial, I'll use [Ninject](http://www.ninject.org/).</span></span> <span data-ttu-id="8552b-187">(Inne popularne biblioteki obejmują [Castle Windsor](http://www.castleproject.org/), [Spring.NET](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity)i [StructureMap](http://docs.structuremap.net)).</span><span class="sxs-lookup"><span data-stu-id="8552b-187">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), and [StructureMap](http://docs.structuremap.net).)</span></span>

<span data-ttu-id="8552b-188">Zainstaluj [bibliotekę Ninject](https://nuget.org/packages/Ninject/3.0.1.10)za pomocą Menedżera pakietów NuGet.</span><span class="sxs-lookup"><span data-stu-id="8552b-188">Use NuGet Package Manager to install the [Ninject library](https://nuget.org/packages/Ninject/3.0.1.10).</span></span> <span data-ttu-id="8552b-189">W programie Visual Studio, w menu **Narzędzia** wybierz pozycję **menedżer pakietów NuGet** > **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="8552b-189">In Visual Studio, from the **Tools** menu select **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="8552b-190">W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="8552b-190">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a><span data-ttu-id="8552b-191">Zastępowanie programu rozpoznawania zależności sygnalizującego</span><span class="sxs-lookup"><span data-stu-id="8552b-191">Replace the SignalR Dependency Resolver</span></span>

<span data-ttu-id="8552b-192">Aby użyć Ninject w ramach sygnalizującego, Utwórz klasę, która pochodzi od **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="8552b-192">To use Ninject within SignalR, create a class that derives from **DefaultDependencyResolver**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

<span data-ttu-id="8552b-193">Ta klasa przesłania metody **GetService** i **GetServices** **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="8552b-193">This class overrides the **GetService** and **GetServices** methods of **DefaultDependencyResolver**.</span></span> <span data-ttu-id="8552b-194">Sygnał wywołuje te metody, aby utworzyć różne obiekty w środowisku uruchomieniowym, w tym wystąpienia centrów, a także różne usługi używane wewnętrznie przez program sygnalizujący.</span><span class="sxs-lookup"><span data-stu-id="8552b-194">SignalR calls these methods to create various objects at runtime, including hub instances, as well as various services used internally by SignalR.</span></span>

- <span data-ttu-id="8552b-195">Metoda **GetService** tworzy pojedyncze wystąpienie typu.</span><span class="sxs-lookup"><span data-stu-id="8552b-195">The **GetService** method creates a single instance of a type.</span></span> <span data-ttu-id="8552b-196">Zastąp tę metodę, aby wywołać metodę **TryGet** jądra Ninject.</span><span class="sxs-lookup"><span data-stu-id="8552b-196">Override this method to call the Ninject kernel's **TryGet** method.</span></span> <span data-ttu-id="8552b-197">Jeśli ta metoda zwróci wartość null, powraca do domyślnego programu rozpoznawania nazw.</span><span class="sxs-lookup"><span data-stu-id="8552b-197">If that method returns null, fall back to the default resolver.</span></span>
- <span data-ttu-id="8552b-198">Metoda **GetServices** tworzy kolekcję obiektów określonego typu.</span><span class="sxs-lookup"><span data-stu-id="8552b-198">The **GetServices** method creates a collection of objects of a specified type.</span></span> <span data-ttu-id="8552b-199">Zastąp tę metodę, aby połączyć wyniki z Ninject z wynikami domyślnego programu rozpoznawania nazw.</span><span class="sxs-lookup"><span data-stu-id="8552b-199">Override this method to concatenate the results from Ninject with the results from the default resolver.</span></span>

## <a name="configure-ninject-bindings"></a><span data-ttu-id="8552b-200">Konfigurowanie powiązań Ninject</span><span class="sxs-lookup"><span data-stu-id="8552b-200">Configure Ninject Bindings</span></span>

<span data-ttu-id="8552b-201">Teraz będziemy używać Ninject do deklarowania powiązań typów.</span><span class="sxs-lookup"><span data-stu-id="8552b-201">Now we'll use Ninject to declare type bindings.</span></span>

<span data-ttu-id="8552b-202">Otwórz klasę Startup.cs aplikacji (utworzoną ręcznie zgodnie z instrukcjami pakietu w `readme.txt`lub utworzoną przez dodanie uwierzytelniania do projektu).</span><span class="sxs-lookup"><span data-stu-id="8552b-202">Open your application's Startup.cs class (that you either created manually as per the package instructions in `readme.txt`, or that was created by adding authentication to your project).</span></span> <span data-ttu-id="8552b-203">W metodzie `Startup.Configuration` Utwórz kontener Ninject, który Ninject wywołuje *jądro*.</span><span class="sxs-lookup"><span data-stu-id="8552b-203">In the `Startup.Configuration` method, create the Ninject container, which Ninject calls the *kernel*.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

<span data-ttu-id="8552b-204">Utwórz wystąpienie naszego niestandardowego programu rozpoznawania zależności:</span><span class="sxs-lookup"><span data-stu-id="8552b-204">Create an instance of our custom dependency resolver:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

<span data-ttu-id="8552b-205">Utwórz powiązanie dla `IStockTicker` w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="8552b-205">Create a binding for `IStockTicker` as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample17.cs)]

<span data-ttu-id="8552b-206">Ten kod zawiera dwie rzeczy.</span><span class="sxs-lookup"><span data-stu-id="8552b-206">This code is saying two things.</span></span> <span data-ttu-id="8552b-207">Po pierwsze, zawsze, gdy aplikacja wymaga `IStockTicker`, jądro powinno utworzyć wystąpienie `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="8552b-207">First, whenever the application needs an `IStockTicker`, the kernel should create an instance of `StockTicker`.</span></span> <span data-ttu-id="8552b-208">Druga klasa `StockTicker` powinna być utworzona jako obiekt pojedynczy.</span><span class="sxs-lookup"><span data-stu-id="8552b-208">Second, the `StockTicker` class should be a created as a singleton object.</span></span> <span data-ttu-id="8552b-209">Ninject utworzy jedno wystąpienie obiektu i zwróci to samo wystąpienie dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="8552b-209">Ninject will create one instance of the object, and return the same instance for each request.</span></span>

<span data-ttu-id="8552b-210">Utwórz powiązanie dla **IHubConnectionContext** w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="8552b-210">Create a binding for **IHubConnectionContext** as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

<span data-ttu-id="8552b-211">Ten kod tworzy funkcję anonimową zwracającą **IHubConnection**.</span><span class="sxs-lookup"><span data-stu-id="8552b-211">This code creates an anonymous function that returns an **IHubConnection**.</span></span> <span data-ttu-id="8552b-212">Metoda **WhenInjectedInto** informuje Ninject o użyciu tej funkcji tylko podczas tworzenia wystąpień `IStockTicker`.</span><span class="sxs-lookup"><span data-stu-id="8552b-212">The **WhenInjectedInto** method tells Ninject to use this function only when creating `IStockTicker` instances.</span></span> <span data-ttu-id="8552b-213">Przyczyną jest to, że sygnalizujący tworzy wewnętrznie wystąpienia **IHubConnectionContext** i nie chcemy przesłonić sposobu tworzenia przez program sygnalizującego.</span><span class="sxs-lookup"><span data-stu-id="8552b-213">The reason is that SignalR creates **IHubConnectionContext** instances internally, and we don't want to override how SignalR creates them.</span></span> <span data-ttu-id="8552b-214">Ta funkcja ma zastosowanie tylko do naszej klasy `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="8552b-214">This function only applies to our `StockTicker` class.</span></span>

<span data-ttu-id="8552b-215">Przekaż program rozpoznawania zależności do metody **MapSignalR** , dodając konfigurację centrum:</span><span class="sxs-lookup"><span data-stu-id="8552b-215">Pass the dependency resolver into the **MapSignalR** method by adding a hub configuration:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

<span data-ttu-id="8552b-216">Zaktualizuj metodę Startup. ConfigureSignalR w klasie startowej przykładu przy użyciu nowego parametru:</span><span class="sxs-lookup"><span data-stu-id="8552b-216">Update the Startup.ConfigureSignalR method in the sample's Startup class with the new parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

<span data-ttu-id="8552b-217">Teraz sygnalizujący będzie używać mechanizmu rozwiązywania konfliktów określonego w **MapSignalR**zamiast domyślnego programu rozpoznawania nazw.</span><span class="sxs-lookup"><span data-stu-id="8552b-217">Now SignalR will use the resolver specified in **MapSignalR**, instead of the default resolver.</span></span>

<span data-ttu-id="8552b-218">Oto kompletna lista kodu dla `Startup.Configuration`.</span><span class="sxs-lookup"><span data-stu-id="8552b-218">Here is the complete code listing for `Startup.Configuration`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample21.cs)]

<span data-ttu-id="8552b-219">Aby uruchomić aplikację StockTicker w programie Visual Studio, naciśnij klawisz F5.</span><span class="sxs-lookup"><span data-stu-id="8552b-219">To run the StockTicker application in Visual Studio, press F5.</span></span> <span data-ttu-id="8552b-220">W oknie przeglądarki przejdź do `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span><span class="sxs-lookup"><span data-stu-id="8552b-220">In the browser window, navigate to `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span></span>

![](dependency-injection/_static/image3.png)

<span data-ttu-id="8552b-221">Aplikacja ma dokładnie te same funkcje jak wcześniej.</span><span class="sxs-lookup"><span data-stu-id="8552b-221">The application has exactly the same functionality as before.</span></span> <span data-ttu-id="8552b-222">(Aby uzyskać opis, zobacz [Server Broadcast with ASP.NET signaler](../getting-started/tutorial-server-broadcast-with-signalr.md)). Zachowanie nie zostało zmienione; znacznie ułatwiają testowanie, konserwację i rozwijanie kodu.</span><span class="sxs-lookup"><span data-stu-id="8552b-222">(For a description, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).) We haven't changed the behavior; just made the code easier to test, maintain, and evolve.</span></span>
