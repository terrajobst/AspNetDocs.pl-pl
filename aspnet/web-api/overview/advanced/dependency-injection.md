---
uid: web-api/overview/advanced/dependency-injection
title: Wstrzykiwanie zależności we wzorcu ASP.NET Web API 2 — ASP.NET 4.x
author: MikeWasson
description: W tym samouczku pokazano, jak wstrzykiwanie zależności do poziomu kontrolera interfejsu API sieci Web platformy ASP.NET dla platformy ASP.NET 4.x.
ms.author: riande
ms.date: 01/20/2014
ms.custom: seoapril2019
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 0ad0b3c63741803e05274df4da3fcbe5481d32a4
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59391939"
---
# <a name="dependency-injection-in-aspnet-web-api-2"></a><span data-ttu-id="c4e6f-103">Wstrzykiwanie zależności we wzorcu ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="c4e6f-103">Dependency Injection in ASP.NET Web API 2</span></span>

<span data-ttu-id="c4e6f-104">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c4e6f-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="c4e6f-105">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="c4e6f-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> <span data-ttu-id="c4e6f-106">W tym samouczku pokazano, jak wstrzykiwanie zależności do poziomu kontrolera interfejsu API sieci Web platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-106">This tutorial shows how to inject dependencies into your ASP.NET Web API controller.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="c4e6f-107">Wersje oprogramowania używanego w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="c4e6f-107">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="c4e6f-108">Internetowy interfejs API 2</span><span class="sxs-lookup"><span data-stu-id="c4e6f-108">Web API 2</span></span>
> - [<span data-ttu-id="c4e6f-109">Blok aplikacji Unity</span><span class="sxs-lookup"><span data-stu-id="c4e6f-109">Unity Application Block</span></span>](https://www.nuget.org/packages/Unity/)
> - <span data-ttu-id="c4e6f-110">Entity Framework 6 (działa także w wersji 5)</span><span class="sxs-lookup"><span data-stu-id="c4e6f-110">Entity Framework 6 (version 5 also works)</span></span>


## <a name="what-is-dependency-injection"></a><span data-ttu-id="c4e6f-111">Co to jest wstrzykiwanie zależności?</span><span class="sxs-lookup"><span data-stu-id="c4e6f-111">What is Dependency Injection?</span></span>

<span data-ttu-id="c4e6f-112">A *zależności* jest dowolny obiekt, który wymaga innego obiektu.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-112">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="c4e6f-113">Na przykład, jest często zdefiniować [repozytorium](http://martinfowler.com/eaaCatalog/repository.html) obsługująca dostęp do danych.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-113">For example, it's common to define a [repository](http://martinfowler.com/eaaCatalog/repository.html) that handles data access.</span></span> <span data-ttu-id="c4e6f-114">Teraz pokazują z przykładem.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-114">Let's illustrate with an example.</span></span> <span data-ttu-id="c4e6f-115">Po pierwsze zdefiniujemy modelu domeny:</span><span class="sxs-lookup"><span data-stu-id="c4e6f-115">First, we'll define a domain model:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="c4e6f-116">W tym miejscu jest klasą prostego repozytorium, która przechowuje elementy w bazie danych przy użyciu platformy Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-116">Here is a simple repository class that stores items in a database, using Entity Framework.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="c4e6f-117">Teraz czynnością jest zdefiniowanie kontroler Web API, który obsługuje żądania GET `Product` jednostek.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-117">Now let's define a Web API controller that supports GET requests for `Product` entities.</span></span> <span data-ttu-id="c4e6f-118">(Czy mam pomijając POST i innych metod dla uproszczenia.) W tym miejscu jest pierwsza próba:</span><span class="sxs-lookup"><span data-stu-id="c4e6f-118">(I'm leaving out POST and other methods for simplicity.) Here is a first attempt:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="c4e6f-119">Należy zauważyć, że klasa kontrolera jest zależna od `ProductRepository`, i zawiadamiamy kontroler tworzenia `ProductRepository` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-119">Notice that the controller class depends on `ProductRepository`, and we are letting the controller create the `ProductRepository` instance.</span></span> <span data-ttu-id="c4e6f-120">Jednak jest to zły pomysł kodowi twardych zależności w ten sposób z kilku powodów.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-120">However, it's a bad idea to hard code the dependency in this way, for several reasons.</span></span>

- <span data-ttu-id="c4e6f-121">Jeśli chcesz zastąpić `ProductRepository` z inną implementacją, trzeba będzie również zmodyfikować klasy kontrolera.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-121">If you want to replace `ProductRepository` with a different implementation, you also need to modify the controller class.</span></span>
- <span data-ttu-id="c4e6f-122">Jeśli `ProductRepository` ma zależności, należy skonfigurować w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-122">If the `ProductRepository` has dependencies, you must configure these inside the controller.</span></span> <span data-ttu-id="c4e6f-123">Dla dużego projektu za pomocą wielu kontrolerów kodu konfiguracji staje się rozproszone w projekcie.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-123">For a large project with multiple controllers, your configuration code becomes scattered across your project.</span></span>
- <span data-ttu-id="c4e6f-124">Ciężko jest testy jednostkowe, ponieważ kontroler jest ustalony do wykonywania zapytań w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-124">It is hard to unit test, because the controller is hard-coded to query the database.</span></span> <span data-ttu-id="c4e6f-125">Dla testu jednostkowego należy użyć pozorny ani klas zastępczych repozytorium, które nie jest możliwe w bieżącym projekcie.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-125">For a unit test, you should use a mock or stub repository, which is not possible with the current design.</span></span>

<span data-ttu-id="c4e6f-126">Można rozwiązać te problemy, zapewniając *wprowadza* repozytorium do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-126">We can address these problems by *injecting* the repository into the controller.</span></span> <span data-ttu-id="c4e6f-127">Po pierwsze, Refaktoryzuj `ProductRepository` klasy w interfejsie:</span><span class="sxs-lookup"><span data-stu-id="c4e6f-127">First, refactor the `ProductRepository` class into an interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="c4e6f-128">Następnie podaj `IProductRepository` jako parametr konstruktora:</span><span class="sxs-lookup"><span data-stu-id="c4e6f-128">Then provide the `IProductRepository` as a constructor parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="c4e6f-129">W tym przykładzie użyto [iniekcji konstruktora](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="c4e6f-129">This example uses [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="c4e6f-130">Można również użyć *iniekcji metody ustawiającej*, gdzie ustawiasz zależności za pomocą metody ustawiającej lub właściwości.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-130">You can also use *setter injection*, where you set the dependency through a setter method or property.</span></span>

<span data-ttu-id="c4e6f-131">Ale teraz występuje problem, ponieważ aplikacja nie tworzy kontroler bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-131">But now there is a problem, because your application doesn't create the controller directly.</span></span> <span data-ttu-id="c4e6f-132">Interfejs API sieci Web tworzy kontroler, kieruje żądanie, gdy interfejs API sieci Web nie nic wiedzieć o `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-132">Web API creates the controller when it routes the request, and Web API doesn't know anything about `IProductRepository`.</span></span> <span data-ttu-id="c4e6f-133">Jest to, gdzie mechanizmu rozpoznawania zależności interfejsu API sieci Web jest dostępna w.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-133">This is where the Web API dependency resolver comes in.</span></span>

## <a name="the-web-api-dependency-resolver"></a><span data-ttu-id="c4e6f-134">Mechanizm rozpoznawania zależności interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="c4e6f-134">The Web API Dependency Resolver</span></span>

<span data-ttu-id="c4e6f-135">Definiuje interfejs API sieci Web **elementu IDependencyResolver** interfejs służący do rozpoznawania zależności.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-135">Web API defines the **IDependencyResolver** interface for resolving dependencies.</span></span> <span data-ttu-id="c4e6f-136">Oto definicja interfejsu:</span><span class="sxs-lookup"><span data-stu-id="c4e6f-136">Here is the definition of the interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="c4e6f-137">**IDependencyScope** interfejs ma dwie metody:</span><span class="sxs-lookup"><span data-stu-id="c4e6f-137">The **IDependencyScope** interface has two methods:</span></span>

- <span data-ttu-id="c4e6f-138">**GetService** tworzy jedno wystąpienie typu.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-138">**GetService** creates one instance of a type.</span></span>
- <span data-ttu-id="c4e6f-139">**Funkcji GetServices** tworzy kolekcję obiektów określonego typu.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-139">**GetServices** creates a collection of objects of a specified type.</span></span>

<span data-ttu-id="c4e6f-140">**Elementu IDependencyResolver** dziedziczy metodę **IDependencyScope** i dodaje **BeginScope** metody.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-140">The **IDependencyResolver** method inherits **IDependencyScope** and adds the **BeginScope** method.</span></span> <span data-ttu-id="c4e6f-141">Omówię zakresów w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-141">I'll talk about scopes later in this tutorial.</span></span>

<span data-ttu-id="c4e6f-142">Kiedy internetowy interfejs API tworzy wystąpienie kontrolera, najpierw wywołuje **IDependencyResolver.GetService**, przekazując typ kontrolera.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-142">When Web API creates a controller instance, it first calls **IDependencyResolver.GetService**, passing in the controller type.</span></span> <span data-ttu-id="c4e6f-143">Tego punktu zaczepienia rozszerzalności umożliwia tworzenie kontrolera, rozwiązywanie wszelkich zależności.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-143">You can use this extensibility hook to create the controller, resolving any dependencies.</span></span> <span data-ttu-id="c4e6f-144">Jeśli **GetService** zwraca wartość null, internetowy interfejs API szuka konstruktorem klasy kontrolera.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-144">If **GetService** returns null, Web API looks for a parameterless constructor on the controller class.</span></span>

## <a name="dependency-resolution-with-the-unity-container"></a><span data-ttu-id="c4e6f-145">Rozpoznawanie zależności za pomocą kontenera aparatu Unity</span><span class="sxs-lookup"><span data-stu-id="c4e6f-145">Dependency Resolution with the Unity Container</span></span>

<span data-ttu-id="c4e6f-146">Mimo że można napisać kompletna **elementu IDependencyResolver** wdrożenia od zera, interfejs naprawdę jest przeznaczony do działania jako Most między interfejsu API sieci Web i istniejące kontenery IoC.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-146">Although you could write a complete **IDependencyResolver** implementation from scratch, the interface is really designed to act as bridge between Web API and existing IoC containers.</span></span>

<span data-ttu-id="c4e6f-147">Kontenera IoC jest składnikiem oprogramowania, który jest odpowiedzialny za zarządzanie zależnościami.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-147">An IoC container is a software component that is responsible for managing dependencies.</span></span> <span data-ttu-id="c4e6f-148">Rejestrowanie typów z kontenerem, a następnie użyć do tworzenia obiektów kontenera.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-148">You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="c4e6f-149">Kontener automatycznie wpadł na pomysł relacji zależności.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-149">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="c4e6f-150">Wiele kontenerów IoC umożliwiają także kontrolowanie elementów, takich jak okres istnienia obiektów i zakresu.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-150">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="c4e6f-151">"IoC" oznacza "Inwersja kontroli", który jest ogólny wzorzec gdzie struktura wywołuje kod aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-151">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="c4e6f-152">Kontenera IoC tworzy obiekty, które zwykle przepływu sterowania "odwraca".</span><span class="sxs-lookup"><span data-stu-id="c4e6f-152">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


<span data-ttu-id="c4e6f-153">W tym samouczku użyjemy [Unity](https://msdn.microsoft.com/library/ff647202.aspx) z Microsoft Patterns &amp; rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-153">For this tutorial, we'll use [Unity](https://msdn.microsoft.com/library/ff647202.aspx) from Microsoft Patterns &amp; Practices.</span></span> <span data-ttu-id="c4e6f-154">(Obejmują innych popularnych bibliotek [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), i [StructureMap ](http://structuremap.github.io/documentation/).) Menedżer pakietów NuGet można użyć do zainstalowania aparatu Unity.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-154">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), and [StructureMap](http://structuremap.github.io/documentation/).) You can use NuGet Package Manager to install Unity.</span></span> <span data-ttu-id="c4e6f-155">Z **narzędzia** menu w programie Visual Studio, wybierz **Menedżera pakietów NuGet**, a następnie wybierz **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-155">From the **Tools** menu in Visual Studio, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="c4e6f-156">W oknie Konsola Menedżera pakietów wpisz następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="c4e6f-156">In the Package Manager Console window, type the following command:</span></span>

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

<span data-ttu-id="c4e6f-157">Oto implementacja **elementu IDependencyResolver** wszystko w kontenerze aparatu Unity.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-157">Here is an implementation of **IDependencyResolver** that wraps a Unity container.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="c4e6f-158">Jeśli **GetService** metody nie można rozpoznać typu, powinna zwrócić **null**.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-158">If the **GetService** method cannot resolve a type, it should return **null**.</span></span> <span data-ttu-id="c4e6f-159">Jeśli **funkcji GetServices** metody nie można rozpoznać typu, powinien zostać zwrócony obiekt pustą kolekcję.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-159">If the **GetServices** method cannot resolve a type, it should return an empty collection object.</span></span> <span data-ttu-id="c4e6f-160">Nie zgłaszają wyjątki dla nieznanego typu.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-160">Don't throw exceptions for unknown types.</span></span>


## <a name="configuring-the-dependency-resolver"></a><span data-ttu-id="c4e6f-161">Konfigurowanie mechanizmu rozpoznawania zależności</span><span class="sxs-lookup"><span data-stu-id="c4e6f-161">Configuring the Dependency Resolver</span></span>

<span data-ttu-id="c4e6f-162">Nastavit mechanizmu rozpoznawania zależności **klasy DependencyResolver** właściwości globalnego **HttpConfiguration** obiektu.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-162">Set the dependency resolver on the **DependencyResolver** property of the global **HttpConfiguration** object.</span></span>

<span data-ttu-id="c4e6f-163">Poniższy kod rejestruje `IProductRepository` interfejs za pomocą aparatu Unity, a następnie tworzy `UnityResolver`.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-163">The following code registers the `IProductRepository` interface with Unity and then creates a `UnityResolver`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a><span data-ttu-id="c4e6f-164">Zakres zależności i okresem istnienia kontrolera</span><span class="sxs-lookup"><span data-stu-id="c4e6f-164">Dependency Scope and Controller Lifetime</span></span>

<span data-ttu-id="c4e6f-165">Kontrolery są tworzone na żądanie.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-165">Controllers are created per request.</span></span> <span data-ttu-id="c4e6f-166">Do zarządzania okresy istnienia obiektu, **elementu IDependencyResolver** korzysta z koncepcji *zakres*.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-166">To manage object lifetimes, **IDependencyResolver** uses the concept of a *scope*.</span></span>

<span data-ttu-id="c4e6f-167">Mechanizm rozpoznawania zależności dołączone do **HttpConfiguration** obiekt ma zakres globalny.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-167">The dependency resolver attached to the **HttpConfiguration** object has global scope.</span></span> <span data-ttu-id="c4e6f-168">Kiedy internetowy interfejs API tworzy kontroler, wywołuje **BeginScope**.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-168">When Web API creates a controller, it calls **BeginScope**.</span></span> <span data-ttu-id="c4e6f-169">Ta metoda zwraca **IDependencyScope** reprezentujący zakresem podrzędnym.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-169">This method returns an **IDependencyScope** that represents a child scope.</span></span>

<span data-ttu-id="c4e6f-170">Następnie wywołuje internetowy interfejs API **GetService** w zakresie podrzędnym, aby utworzyć kontroler.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-170">Web API then calls **GetService** on the child scope to create the controller.</span></span> <span data-ttu-id="c4e6f-171">Po zakończeniu żądania wywołania interfejsu API sieci Web **Dispose** w zakresie podrzędnym.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-171">When request is complete, Web API calls **Dispose** on the child scope.</span></span> <span data-ttu-id="c4e6f-172">Użyj **Dispose** metodę dispose zależności kontrolera.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-172">Use the **Dispose** method to dispose of the controller's dependencies.</span></span>

<span data-ttu-id="c4e6f-173">Jak zaimplementować **BeginScope** zależy od kontenera IoC.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-173">How you implement **BeginScope** depends on the IoC container.</span></span> <span data-ttu-id="c4e6f-174">Zakres dla aparatu Unity, odnosi się do kontenerów podrzędnych:</span><span class="sxs-lookup"><span data-stu-id="c4e6f-174">For Unity, scope corresponds to a child container:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

<span data-ttu-id="c4e6f-175">Większość kontenery IoC mają podobne odpowiedniki.</span><span class="sxs-lookup"><span data-stu-id="c4e6f-175">Most IoC containers have similar equivalents.</span></span>
