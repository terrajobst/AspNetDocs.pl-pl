---
uid: web-api/overview/advanced/dependency-injection
title: Iniekcja zależności w ASP.NET Web API 2-ASP.NET 4. x
author: MikeWasson
description: W tym samouczku pokazano, jak wstrzyknąć zależności do kontrolera internetowego interfejsu API ASP.NET dla ASP.NET 4. x.
ms.author: riande
ms.date: 01/20/2014
ms.custom: seoapril2019
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: f9c212af92168ac02644625b9aa8ec1bef329cab
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622606"
---
# <a name="dependency-injection-in-aspnet-web-api-2"></a><span data-ttu-id="65f52-103">Iniekcja zależności w ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="65f52-103">Dependency Injection in ASP.NET Web API 2</span></span>

<span data-ttu-id="65f52-104">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="65f52-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="65f52-105">Pobierz ukończony projekt</span><span class="sxs-lookup"><span data-stu-id="65f52-105">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> <span data-ttu-id="65f52-106">W tym samouczku pokazano, jak wstrzyknąć zależności do kontrolera interfejsu API sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="65f52-106">This tutorial shows how to inject dependencies into your ASP.NET Web API controller.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="65f52-107">Wersje oprogramowania używane w samouczku</span><span class="sxs-lookup"><span data-stu-id="65f52-107">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="65f52-108">Internetowy interfejs API 2</span><span class="sxs-lookup"><span data-stu-id="65f52-108">Web API 2</span></span>
> - [<span data-ttu-id="65f52-109">Blok aplikacji aparatu Unity</span><span class="sxs-lookup"><span data-stu-id="65f52-109">Unity Application Block</span></span>](https://www.nuget.org/packages/Unity/)
> - <span data-ttu-id="65f52-110">Entity Framework 6 (wersja 5 działa również)</span><span class="sxs-lookup"><span data-stu-id="65f52-110">Entity Framework 6 (version 5 also works)</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="65f52-111">Co to jest iniekcja zależności?</span><span class="sxs-lookup"><span data-stu-id="65f52-111">What is Dependency Injection?</span></span>

<span data-ttu-id="65f52-112">*Zależność* jest dowolnym obiektem, który jest wymagany przez inny obiekt.</span><span class="sxs-lookup"><span data-stu-id="65f52-112">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="65f52-113">Na przykład często istnieje możliwość zdefiniowania [repozytorium](http://martinfowler.com/eaaCatalog/repository.html) , które obsługuje dostęp do danych.</span><span class="sxs-lookup"><span data-stu-id="65f52-113">For example, it's common to define a [repository](http://martinfowler.com/eaaCatalog/repository.html) that handles data access.</span></span> <span data-ttu-id="65f52-114">Ilustrujemy przykład.</span><span class="sxs-lookup"><span data-stu-id="65f52-114">Let's illustrate with an example.</span></span> <span data-ttu-id="65f52-115">Najpierw zdefiniujemy model domeny:</span><span class="sxs-lookup"><span data-stu-id="65f52-115">First, we'll define a domain model:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="65f52-116">Oto prosta Klasa repozytorium, która przechowuje elementy w bazie danych przy użyciu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="65f52-116">Here is a simple repository class that stores items in a database, using Entity Framework.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="65f52-117">Teraz zdefiniujemy kontroler interfejsu API sieci Web, który obsługuje żądania GET dla jednostek `Product`.</span><span class="sxs-lookup"><span data-stu-id="65f52-117">Now let's define a Web API controller that supports GET requests for `Product` entities.</span></span> <span data-ttu-id="65f52-118">(Pozostawiam wpis i inne metody dla uproszczenia). Oto pierwsza próba:</span><span class="sxs-lookup"><span data-stu-id="65f52-118">(I'm leaving out POST and other methods for simplicity.) Here is a first attempt:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="65f52-119">Należy zauważyć, że klasa kontrolera zależy od `ProductRepository`i zezwalamy kontrolerowi na tworzenie wystąpienia `ProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="65f52-119">Notice that the controller class depends on `ProductRepository`, and we are letting the controller create the `ProductRepository` instance.</span></span> <span data-ttu-id="65f52-120">Niemniej jednak dobrym pomysłem jest napełnienie kodu zależności w ten sposób, z kilku powodów.</span><span class="sxs-lookup"><span data-stu-id="65f52-120">However, it's a bad idea to hard code the dependency in this way, for several reasons.</span></span>

- <span data-ttu-id="65f52-121">Aby zastąpić `ProductRepository` z inną implementacją, należy również zmodyfikować klasę kontrolera.</span><span class="sxs-lookup"><span data-stu-id="65f52-121">If you want to replace `ProductRepository` with a different implementation, you also need to modify the controller class.</span></span>
- <span data-ttu-id="65f52-122">Jeśli `ProductRepository` ma zależności, należy je skonfigurować w ramach kontrolera.</span><span class="sxs-lookup"><span data-stu-id="65f52-122">If the `ProductRepository` has dependencies, you must configure these inside the controller.</span></span> <span data-ttu-id="65f52-123">W przypadku dużego projektu z wieloma kontrolerami kod konfiguracji jest rozmieszczany w całym projekcie.</span><span class="sxs-lookup"><span data-stu-id="65f52-123">For a large project with multiple controllers, your configuration code becomes scattered across your project.</span></span>
- <span data-ttu-id="65f52-124">Test jednostkowy jest trudno, ponieważ jest on zakodowany do wykonywania zapytań w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="65f52-124">It is hard to unit test, because the controller is hard-coded to query the database.</span></span> <span data-ttu-id="65f52-125">W przypadku testów jednostkowych należy użyć repozytorium makiety lub stub, które nie jest możliwe w bieżącym projekcie.</span><span class="sxs-lookup"><span data-stu-id="65f52-125">For a unit test, you should use a mock or stub repository, which is not possible with the current design.</span></span>

<span data-ttu-id="65f52-126">Możemy rozwiązać te problemy poprzez *wstrzyknięcie* repozytorium do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="65f52-126">We can address these problems by *injecting* the repository into the controller.</span></span> <span data-ttu-id="65f52-127">Najpierw Refaktoryzacja klasy `ProductRepository` w interfejsie:</span><span class="sxs-lookup"><span data-stu-id="65f52-127">First, refactor the `ProductRepository` class into an interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="65f52-128">Następnie podaj `IProductRepository` jako parametr konstruktora:</span><span class="sxs-lookup"><span data-stu-id="65f52-128">Then provide the `IProductRepository` as a constructor parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="65f52-129">W tym przykładzie zastosowano [iniekcję konstruktora](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="65f52-129">This example uses [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="65f52-130">Można również użyć *iniekcji setter*, gdzie ustawia się zależność za pomocą metody lub właściwości setter.</span><span class="sxs-lookup"><span data-stu-id="65f52-130">You can also use *setter injection*, where you set the dependency through a setter method or property.</span></span>

<span data-ttu-id="65f52-131">Jednak teraz występuje problem, ponieważ aplikacja nie tworzy kontrolera bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="65f52-131">But now there is a problem, because your application doesn't create the controller directly.</span></span> <span data-ttu-id="65f52-132">Interfejs API sieci Web tworzy kontroler, gdy kieruje żądanie, a interfejs API sieci Web nie wie niczego o `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="65f52-132">Web API creates the controller when it routes the request, and Web API doesn't know anything about `IProductRepository`.</span></span> <span data-ttu-id="65f52-133">Jest to miejsce, w którym znajduje się program rozpoznawania zależności interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="65f52-133">This is where the Web API dependency resolver comes in.</span></span>

## <a name="the-web-api-dependency-resolver"></a><span data-ttu-id="65f52-134">Program rozpoznawania zależności interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="65f52-134">The Web API Dependency Resolver</span></span>

<span data-ttu-id="65f52-135">Internetowy interfejs API definiuje interfejs **IDependencyResolver** do rozpoznawania zależności.</span><span class="sxs-lookup"><span data-stu-id="65f52-135">Web API defines the **IDependencyResolver** interface for resolving dependencies.</span></span> <span data-ttu-id="65f52-136">Oto definicja interfejsu:</span><span class="sxs-lookup"><span data-stu-id="65f52-136">Here is the definition of the interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="65f52-137">Interfejs **IDependencyScope** ma dwie metody:</span><span class="sxs-lookup"><span data-stu-id="65f52-137">The **IDependencyScope** interface has two methods:</span></span>

- <span data-ttu-id="65f52-138">**GetService** tworzy jedno wystąpienie typu.</span><span class="sxs-lookup"><span data-stu-id="65f52-138">**GetService** creates one instance of a type.</span></span>
- <span data-ttu-id="65f52-139">**GetServices** tworzy kolekcję obiektów określonego typu.</span><span class="sxs-lookup"><span data-stu-id="65f52-139">**GetServices** creates a collection of objects of a specified type.</span></span>

<span data-ttu-id="65f52-140">Metoda **IDependencyResolver** dziedziczy **IDependencyScope** i dodaje metodę **BeginScope** .</span><span class="sxs-lookup"><span data-stu-id="65f52-140">The **IDependencyResolver** method inherits **IDependencyScope** and adds the **BeginScope** method.</span></span> <span data-ttu-id="65f52-141">Porozmawiamy o zakresach w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="65f52-141">I'll talk about scopes later in this tutorial.</span></span>

<span data-ttu-id="65f52-142">Gdy interfejs API sieci Web tworzy wystąpienie kontrolera, najpierw wywołuje **IDependencyResolver. GetService**, przekazując do typu kontrolera.</span><span class="sxs-lookup"><span data-stu-id="65f52-142">When Web API creates a controller instance, it first calls **IDependencyResolver.GetService**, passing in the controller type.</span></span> <span data-ttu-id="65f52-143">Za pomocą tego punktu zaczepienia rozszerzalności można utworzyć kontroler, rozwiązując wszystkie zależności.</span><span class="sxs-lookup"><span data-stu-id="65f52-143">You can use this extensibility hook to create the controller, resolving any dependencies.</span></span> <span data-ttu-id="65f52-144">Jeśli **GetService** zwraca wartość null, interfejs API sieci Web szuka konstruktora bez parametrów w klasie Controller.</span><span class="sxs-lookup"><span data-stu-id="65f52-144">If **GetService** returns null, Web API looks for a parameterless constructor on the controller class.</span></span>

## <a name="dependency-resolution-with-the-unity-container"></a><span data-ttu-id="65f52-145">Rozpoznawanie zależności przy użyciu kontenera Unity</span><span class="sxs-lookup"><span data-stu-id="65f52-145">Dependency Resolution with the Unity Container</span></span>

<span data-ttu-id="65f52-146">Chociaż można napisać kompletną implementację **IDependencyResolver** od podstaw, interfejs jest naprawdę zaprojektowany do działania jako Most między interfejsem API sieci Web a istniejącymi kontenerami IOC.</span><span class="sxs-lookup"><span data-stu-id="65f52-146">Although you could write a complete **IDependencyResolver** implementation from scratch, the interface is really designed to act as bridge between Web API and existing IoC containers.</span></span>

<span data-ttu-id="65f52-147">Kontener IoC to składnik oprogramowania, który jest odpowiedzialny za zarządzanie zależnościami.</span><span class="sxs-lookup"><span data-stu-id="65f52-147">An IoC container is a software component that is responsible for managing dependencies.</span></span> <span data-ttu-id="65f52-148">Możesz zarejestrować typy w kontenerze, a następnie użyć kontenera do tworzenia obiektów.</span><span class="sxs-lookup"><span data-stu-id="65f52-148">You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="65f52-149">Kontener automatycznie określa relacje zależności.</span><span class="sxs-lookup"><span data-stu-id="65f52-149">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="65f52-150">Wiele kontenerów IoC umożliwia również Sterowanie elementami, takimi jak okres istnienia obiektu i zakres.</span><span class="sxs-lookup"><span data-stu-id="65f52-150">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="65f52-151">"IoC" oznacza "Inversion of Control", który jest ogólnym wzorcem, w którym struktura wywołuje kod aplikacji.</span><span class="sxs-lookup"><span data-stu-id="65f52-151">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="65f52-152">Kontener IoC konstruuje obiekty, co oznacza, że jest to normalny przepływ sterowania.</span><span class="sxs-lookup"><span data-stu-id="65f52-152">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>

<span data-ttu-id="65f52-153">W tym samouczku będziemy używać [aparatu Unity](https://msdn.microsoft.com/library/ff647202.aspx) firmy Microsoft dla &amp; praktyk.</span><span class="sxs-lookup"><span data-stu-id="65f52-153">For this tutorial, we'll use [Unity](https://msdn.microsoft.com/library/ff647202.aspx) from Microsoft Patterns &amp; Practices.</span></span> <span data-ttu-id="65f52-154">(Inne popularne biblioteki obejmują [Castle Windsor](http://www.castleproject.org/), [Spring.NET](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/)i [StructureMap](http://structuremap.github.io/documentation/)). Do zainstalowania aparatu Unity można użyć Menedżera pakietów NuGet.</span><span class="sxs-lookup"><span data-stu-id="65f52-154">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), and [StructureMap](http://structuremap.github.io/documentation/).) You can use NuGet Package Manager to install Unity.</span></span> <span data-ttu-id="65f52-155">W menu **Narzędzia** w programie Visual Studio wybierz pozycję **Menedżer pakietów NuGet**, a następnie wybierz pozycję **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="65f52-155">From the **Tools** menu in Visual Studio, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="65f52-156">W oknie Konsola Menedżera pakietów wpisz następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="65f52-156">In the Package Manager Console window, type the following command:</span></span>

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

<span data-ttu-id="65f52-157">Oto implementacja **IDependencyResolver** , która otacza kontener Unity.</span><span class="sxs-lookup"><span data-stu-id="65f52-157">Here is an implementation of **IDependencyResolver** that wraps a Unity container.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="65f52-158">Jeśli metoda **GetService** nie może rozpoznać typu, powinien zwrócić **wartość null**.</span><span class="sxs-lookup"><span data-stu-id="65f52-158">If the **GetService** method cannot resolve a type, it should return **null**.</span></span> <span data-ttu-id="65f52-159">Jeśli metoda **GetServices** nie może rozpoznać typu, powinien zwrócić pusty obiekt kolekcji.</span><span class="sxs-lookup"><span data-stu-id="65f52-159">If the **GetServices** method cannot resolve a type, it should return an empty collection object.</span></span> <span data-ttu-id="65f52-160">Nie zgłaszaj wyjątków dla nieznanych typów.</span><span class="sxs-lookup"><span data-stu-id="65f52-160">Don't throw exceptions for unknown types.</span></span>

## <a name="configuring-the-dependency-resolver"></a><span data-ttu-id="65f52-161">Konfigurowanie programu rozpoznawania zależności</span><span class="sxs-lookup"><span data-stu-id="65f52-161">Configuring the Dependency Resolver</span></span>

<span data-ttu-id="65f52-162">Ustaw program rozpoznawania zależności we właściwości **DependencyResolver** globalnego obiektu **HttpConfiguration** .</span><span class="sxs-lookup"><span data-stu-id="65f52-162">Set the dependency resolver on the **DependencyResolver** property of the global **HttpConfiguration** object.</span></span>

<span data-ttu-id="65f52-163">Poniższy kod rejestruje interfejs `IProductRepository` przy użyciu aparatu Unity, a następnie tworzy `UnityResolver`.</span><span class="sxs-lookup"><span data-stu-id="65f52-163">The following code registers the `IProductRepository` interface with Unity and then creates a `UnityResolver`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a><span data-ttu-id="65f52-164">Zakres zależności i okres istnienia kontrolera</span><span class="sxs-lookup"><span data-stu-id="65f52-164">Dependency Scope and Controller Lifetime</span></span>

<span data-ttu-id="65f52-165">Na żądanie są tworzone kontrolery.</span><span class="sxs-lookup"><span data-stu-id="65f52-165">Controllers are created per request.</span></span> <span data-ttu-id="65f52-166">Aby zarządzać okresami istnienia obiektów, **IDependencyResolver** używa koncepcji *zakresu*.</span><span class="sxs-lookup"><span data-stu-id="65f52-166">To manage object lifetimes, **IDependencyResolver** uses the concept of a *scope*.</span></span>

<span data-ttu-id="65f52-167">Program rozpoznawania zależności dołączony do obiektu **HttpConfiguration** ma zakres globalny.</span><span class="sxs-lookup"><span data-stu-id="65f52-167">The dependency resolver attached to the **HttpConfiguration** object has global scope.</span></span> <span data-ttu-id="65f52-168">Gdy interfejs API sieci Web tworzy kontroler, wywołuje **BeginScope**.</span><span class="sxs-lookup"><span data-stu-id="65f52-168">When Web API creates a controller, it calls **BeginScope**.</span></span> <span data-ttu-id="65f52-169">Ta metoda zwraca **IDependencyScope** , który reprezentuje zakres podrzędny.</span><span class="sxs-lookup"><span data-stu-id="65f52-169">This method returns an **IDependencyScope** that represents a child scope.</span></span>

<span data-ttu-id="65f52-170">Interfejs API sieci Web następnie wywołuje metodę **GetService** w zakresie podrzędnym, aby utworzyć kontroler.</span><span class="sxs-lookup"><span data-stu-id="65f52-170">Web API then calls **GetService** on the child scope to create the controller.</span></span> <span data-ttu-id="65f52-171">Po zakończeniu żądania interfejs API sieci Web wywołuje metodę **Dispose** dla zakresu podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="65f52-171">When request is complete, Web API calls **Dispose** on the child scope.</span></span> <span data-ttu-id="65f52-172">Użyj metody **Dispose** , aby usunąć zależności kontrolera.</span><span class="sxs-lookup"><span data-stu-id="65f52-172">Use the **Dispose** method to dispose of the controller's dependencies.</span></span>

<span data-ttu-id="65f52-173">Sposób implementacji **BeginScope** zależy od kontenera IOC.</span><span class="sxs-lookup"><span data-stu-id="65f52-173">How you implement **BeginScope** depends on the IoC container.</span></span> <span data-ttu-id="65f52-174">W przypadku aparatu Unity zakres odpowiada kontenerowi podrzędnemu:</span><span class="sxs-lookup"><span data-stu-id="65f52-174">For Unity, scope corresponds to a child container:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

<span data-ttu-id="65f52-175">Większość kontenerów IoC ma podobne odpowiedniki.</span><span class="sxs-lookup"><span data-stu-id="65f52-175">Most IoC containers have similar equivalents.</span></span>
