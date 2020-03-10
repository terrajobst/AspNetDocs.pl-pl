---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: Wstrzykiwanie zależności ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: 'Uwaga: w tym ćwiczeniu praktycznym założono, że masz podstawową wiedzę na temat filtrów ASP.NET MVC i ASP.NET MVC 4. Jeśli nie korzystasz wcześniej z filtrów ASP.NET MVC 4, Rec...'
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 15c9d4dcb9e2c6b9f6adf54d65d15737b32cca3b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78560614"
---
# <a name="aspnet-mvc-4-dependency-injection"></a><span data-ttu-id="cce2f-104">ASP.NET MVC 4 — wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="cce2f-104">ASP.NET MVC 4 Dependency Injection</span></span>

<span data-ttu-id="cce2f-105">przez [zespół Camp sieci Web](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="cce2f-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="cce2f-106">Pobierz zestaw szkoleniowy dla sieci Web Camp</span><span class="sxs-lookup"><span data-stu-id="cce2f-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="cce2f-107">W tym laboratorium praktycznym założono, że masz podstawową wiedzę na temat **ASP.NET MVC** i **ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="cce2f-107">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC** and **ASP.NET MVC 4 filters**.</span></span> <span data-ttu-id="cce2f-108">Jeśli wcześniej nie korzystasz z **filtrów ASP.NET MVC 4** , zalecamy przechodzenie do programu **ASP.NETej akcji niestandardowej MVC** .</span><span class="sxs-lookup"><span data-stu-id="cce2f-108">If you have not used **ASP.NET MVC 4 filters** before, we recommend you to go over **ASP.NET MVC Custom Action Filters** Hands-on Lab.</span></span>

> [!NOTE]
> <span data-ttu-id="cce2f-109">Wszystkie przykładowe kod i fragmenty kodu są dołączone do zestawu szkoleniowego dla sieci Web Camp, dostępnego w [wersji Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="cce2f-109">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="cce2f-110">Projekt specyficzny dla tego laboratorium jest dostępny pod kątem [ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="cce2f-110">The project specific to this lab is available at [ASP.NET MVC 4 Dependency Injection](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).</span></span>

<span data-ttu-id="cce2f-111">W modelu **programowania zorientowanego obiektowo** obiekty współpracują z modelem współpracy, w którym znajdują się Współautorzy i konsumenci.</span><span class="sxs-lookup"><span data-stu-id="cce2f-111">In **Object Oriented Programming** paradigm, objects work together in a collaboration model where there are contributors and consumers.</span></span> <span data-ttu-id="cce2f-112">Naturalnie ten model komunikacji generuje zależności między obiektami i składnikami, co utrudnia zarządzanie w przypadku wzrostu złożoności.</span><span class="sxs-lookup"><span data-stu-id="cce2f-112">Naturally, this communication model generates dependencies between objects and components, becoming difficult to manage when complexity increases.</span></span>

<span data-ttu-id="cce2f-113">![Zależności klas i złożoność modelu](aspnet-mvc-4-dependency-injection/_static/image1.png "Zależności klas i złożoność modelu")</span><span class="sxs-lookup"><span data-stu-id="cce2f-113">![Class dependencies and model complexity](aspnet-mvc-4-dependency-injection/_static/image1.png "Class dependencies and model complexity")</span></span>

<span data-ttu-id="cce2f-114">*Zależności klas i złożoność modelu*</span><span class="sxs-lookup"><span data-stu-id="cce2f-114">*Class dependencies and model complexity*</span></span>

<span data-ttu-id="cce2f-115">Prawdopodobnie wiesz o **wzorcu fabryki** i rozdzieleniu między interfejsem a implementacją przy użyciu usług, gdzie obiekty klienckie są często odpowiedzialne za lokalizację usługi.</span><span class="sxs-lookup"><span data-stu-id="cce2f-115">You have probably heard about the **Factory Pattern** and the separation between the interface and the implementation using services, where the client objects are often responsible for service location.</span></span>

<span data-ttu-id="cce2f-116">Wzorzec iniekcji zależności jest konkretną implementacją nieużywanej kontroli.</span><span class="sxs-lookup"><span data-stu-id="cce2f-116">The Dependency Injection pattern is a particular implementation of Inversion of Control.</span></span> <span data-ttu-id="cce2f-117">**Inversion of Control (IOC)** oznacza, że obiekty nie tworzą innych obiektów, na których polegają na wykonaniu ich pracy.</span><span class="sxs-lookup"><span data-stu-id="cce2f-117">**Inversion of Control (IoC)** means that objects do not create other objects on which they rely to do their work.</span></span> <span data-ttu-id="cce2f-118">Zamiast tego uzyskują wymagane obiekty ze źródła zewnętrznego (na przykład plik konfiguracyjny XML).</span><span class="sxs-lookup"><span data-stu-id="cce2f-118">Instead, they get the objects that they need from an outside source (for example, an xml configuration file).</span></span>

<span data-ttu-id="cce2f-119">**Iniekcja zależności (di)** oznacza, że jest to wykonywane bez interwencji obiektu, zazwyczaj przez składnik platformy, który przekazuje parametry konstruktora i ustawia właściwości.</span><span class="sxs-lookup"><span data-stu-id="cce2f-119">**Dependency Injection (DI)** means that this is done without the object intervention, usually by a framework component that passes constructor parameters and set properties.</span></span>

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a><span data-ttu-id="cce2f-120">Wzorzec projektowania iniekcji zależności (DI)</span><span class="sxs-lookup"><span data-stu-id="cce2f-120">The Dependency Injection (DI) Design Pattern</span></span>

<span data-ttu-id="cce2f-121">Na wysokim poziomie, celem iniekcji zależności jest to, że Klasa klienta (np. *Golf*) potrzebuje elementu, który spełnia interfejs (np. *IClub*).</span><span class="sxs-lookup"><span data-stu-id="cce2f-121">At a high level, the goal of Dependency Injection is that a client class (e.g. *the golfer*) needs something that satisfies an interface (e.g. *IClub*).</span></span> <span data-ttu-id="cce2f-122">Nie ma opieki, jaki jest konkretny typ (np. *WoodClub, IronClub, WedgeClub* lub *PutterClub*), chce, aby ktoś inny obsłużył to (np. dobry *Caddy*).</span><span class="sxs-lookup"><span data-stu-id="cce2f-122">It doesn't care what the concrete type is (e.g. *WoodClub, IronClub, WedgeClub* or *PutterClub*), it wants someone else to handle that (e.g. a good *caddy*).</span></span> <span data-ttu-id="cce2f-123">Program rozpoznawania zależności w ASP.NET MVC umożliwia zarejestrowanie logiki zależności w innym miejscu (np. w kontenerze lub w *torbie Trefl*).</span><span class="sxs-lookup"><span data-stu-id="cce2f-123">The Dependency Resolver in ASP.NET MVC can allow you to register your dependency logic somewhere else (e.g. a container or a *bag of clubs*).</span></span>

<span data-ttu-id="cce2f-124">![Diagram iniekcji zależności](aspnet-mvc-4-dependency-injection/_static/image2.png "Ilustracja iniekcji zależności")</span><span class="sxs-lookup"><span data-stu-id="cce2f-124">![Dependency Injection diagram](aspnet-mvc-4-dependency-injection/_static/image2.png "Dependency Injection illustration")</span></span>

<span data-ttu-id="cce2f-125">*Wstrzykiwanie zależności — analogowe Źródło golfa*</span><span class="sxs-lookup"><span data-stu-id="cce2f-125">*Dependency Injection - Golf analogy*</span></span>

<span data-ttu-id="cce2f-126">Zalety używania wzorca iniekcji zależności i Inwersja kontroli są następujące:</span><span class="sxs-lookup"><span data-stu-id="cce2f-126">The advantages of using Dependency Injection pattern and Inversion of Control are the following:</span></span>

- <span data-ttu-id="cce2f-127">Redukuje sprzęganie klas</span><span class="sxs-lookup"><span data-stu-id="cce2f-127">Reduces class coupling</span></span>
- <span data-ttu-id="cce2f-128">Zwiększa użycie kodu</span><span class="sxs-lookup"><span data-stu-id="cce2f-128">Increases code reusing</span></span>
- <span data-ttu-id="cce2f-129">Zwiększa łatwość utrzymania kodu</span><span class="sxs-lookup"><span data-stu-id="cce2f-129">Improves code maintainability</span></span>
- <span data-ttu-id="cce2f-130">Usprawnia testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="cce2f-130">Improves application testing</span></span>

> [!NOTE]
> <span data-ttu-id="cce2f-131">Wstrzykiwanie zależności jest czasami porównywane z wzorcem wzorca konstrukcji abstrakcyjnej, ale istnieje niewielka różnica między obydwoma podejściami.</span><span class="sxs-lookup"><span data-stu-id="cce2f-131">Dependency Injection is sometimes compared with Abstract Factory Design Pattern, but there is a slight difference between both approaches.</span></span> <span data-ttu-id="cce2f-132">Program DI ma strukturę działającą w celu rozwiązywania zależności przez wywoływanie fabryk i zarejestrowanych usług.</span><span class="sxs-lookup"><span data-stu-id="cce2f-132">DI has a Framework working behind to solve dependencies by calling the factories and the registered services.</span></span>

<span data-ttu-id="cce2f-133">Teraz, po zrozumieniu wzorca iniekcji zależności, dowiesz się, jak go zastosować w ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="cce2f-133">Now that you understand the Dependency Injection Pattern, you will learn throughout this lab how to apply it in ASP.NET MVC 4.</span></span> <span data-ttu-id="cce2f-134">Aby włączyć usługę dostępu do bazy danych, należy rozpocząć korzystanie z iniekcji zależności na **kontrolerach** .</span><span class="sxs-lookup"><span data-stu-id="cce2f-134">You will start using Dependency Injection in the **Controllers** to include a database access service.</span></span> <span data-ttu-id="cce2f-135">Następnie należy zastosować iniekcję zależności do **widoków** w celu użycia usługi i wyświetlenia informacji.</span><span class="sxs-lookup"><span data-stu-id="cce2f-135">Next, you will apply Dependency Injection to the **Views** to consume a service and show information.</span></span> <span data-ttu-id="cce2f-136">Na koniec należy rozszerzyć filtry DI to ASP.NET MVC 4, wstawiając niestandardowy filtr akcji do rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="cce2f-136">Finally, you will extend the DI to ASP.NET MVC 4 Filters, injecting a custom action filter in the solution.</span></span>

<span data-ttu-id="cce2f-137">W tym ćwiczeniu dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="cce2f-137">In this Hands-on Lab, you will learn how to:</span></span>

- <span data-ttu-id="cce2f-138">Integrowanie ASP.NET MVC 4 z Unity dla iniekcji zależności przy użyciu pakietów NuGet</span><span class="sxs-lookup"><span data-stu-id="cce2f-138">Integrate ASP.NET MVC 4 with Unity for Dependency Injection using NuGet Packages</span></span>
- <span data-ttu-id="cce2f-139">Używanie iniekcji zależności wewnątrz kontrolera ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="cce2f-139">Use Dependency Injection inside an ASP.NET MVC Controller</span></span>
- <span data-ttu-id="cce2f-140">Używanie iniekcji zależności wewnątrz widoku ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="cce2f-140">Use Dependency Injection inside an ASP.NET MVC View</span></span>
- <span data-ttu-id="cce2f-141">Użyj iniekcji zależności wewnątrz filtru akcji ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="cce2f-141">Use Dependency Injection inside an ASP.NET MVC Action Filter</span></span>

> [!NOTE]
> <span data-ttu-id="cce2f-142">W tym laboratorium jest używany pakiet NuGet Unity. Mvc3 do rozpoznawania zależności, ale możliwe jest dostosowanie wszelkich platform iniekcji zależności do współdziałania z ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="cce2f-142">This Lab is using Unity.Mvc3 NuGet Package for dependency resolution, but it is possible to adapt any Dependency Injection Framework to work with ASP.NET MVC 4.</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="cce2f-143">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="cce2f-143">Prerequisites</span></span>

<span data-ttu-id="cce2f-144">Aby ukończyć to laboratorium, musisz mieć następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="cce2f-144">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="cce2f-145">[Microsoft Visual Studio Express 2012 dla sieci Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub nadrzędnych (Przeczytaj [dodatek A,](#AppendixA) Aby uzyskać instrukcje na temat sposobu jego instalacji).</span><span class="sxs-lookup"><span data-stu-id="cce2f-145">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="cce2f-146">Konfigurowanie</span><span class="sxs-lookup"><span data-stu-id="cce2f-146">Setup</span></span>

<span data-ttu-id="cce2f-147">**Instalowanie fragmentów kodu**</span><span class="sxs-lookup"><span data-stu-id="cce2f-147">**Installing Code Snippets**</span></span>

<span data-ttu-id="cce2f-148">Dla wygody większość kodu, który będzie zarządzany w tym laboratorium, jest dostępna jako fragmenty kodu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cce2f-148">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="cce2f-149">Aby zainstalować plik fragmentów kodu, uruchom **.\Source\Setup\CodeSnippets.VSI** .</span><span class="sxs-lookup"><span data-stu-id="cce2f-149">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="cce2f-150">Jeśli nie znasz fragmentów kodu Visual Studio Code i chcesz dowiedzieć się, jak z nich korzystać, możesz odwołać się do dodatku z tego dokumentu &quot;[dodatku B: Using fragmenty kodu](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="cce2f-150">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="cce2f-151">Symulacyjn</span><span class="sxs-lookup"><span data-stu-id="cce2f-151">Exercises</span></span>

<span data-ttu-id="cce2f-152">To laboratorium praktyczne obejmuje następujące ćwiczenia:</span><span class="sxs-lookup"><span data-stu-id="cce2f-152">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="cce2f-153">Ćwiczenie 1: wstrzyknięcie kontrolera</span><span class="sxs-lookup"><span data-stu-id="cce2f-153">Exercise 1: Injecting a Controller</span></span>](#Exercise1)
2. [<span data-ttu-id="cce2f-154">Ćwiczenie 2: wprowadzanie widoku</span><span class="sxs-lookup"><span data-stu-id="cce2f-154">Exercise 2: Injecting a View</span></span>](#Exercise2)
3. [<span data-ttu-id="cce2f-155">Ćwiczenie 3: wprowadzanie filtrów</span><span class="sxs-lookup"><span data-stu-id="cce2f-155">Exercise 3: Injecting Filters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="cce2f-156">Każdemu ćwiczeniu towarzyszy folder **końcowy** zawierający rozwiązanie, które należy uzyskać po zakończeniu ćwiczenia.</span><span class="sxs-lookup"><span data-stu-id="cce2f-156">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="cce2f-157">Tego rozwiązania można użyć jako przewodnika, jeśli potrzebujesz dodatkowej pomocy w pracy przez ćwiczenia.</span><span class="sxs-lookup"><span data-stu-id="cce2f-157">You can use this solution as a guide if you need additional help working through the exercises.</span></span>

<span data-ttu-id="cce2f-158">Szacowany czas wykonywania tego laboratorium: **30 minut**.</span><span class="sxs-lookup"><span data-stu-id="cce2f-158">Estimated time to complete this lab: **30 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a><span data-ttu-id="cce2f-159">Ćwiczenie 1: wstrzyknięcie kontrolera</span><span class="sxs-lookup"><span data-stu-id="cce2f-159">Exercise 1: Injecting a Controller</span></span>

<span data-ttu-id="cce2f-160">W tym ćwiczeniu dowiesz się, jak używać iniekcji zależności w kontrolerach ASP.NET MVC przez integrację aparatu Unity z użyciem pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="cce2f-160">In this exercise, you will learn how to use Dependency Injection in ASP.NET MVC Controllers by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="cce2f-161">Z tego powodu usługi są uwzględniane w kontrolerach MvcMusicStore w celu oddzielenia logiki od dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="cce2f-161">For that reason, you will include services into your MvcMusicStore controllers to separate the logic from the data access.</span></span> <span data-ttu-id="cce2f-162">Usługi spowodują utworzenie nowej zależności w konstruktorze kontrolera, który zostanie rozwiązany przy użyciu iniekcji zależności przy pomocy **aparatu Unity**.</span><span class="sxs-lookup"><span data-stu-id="cce2f-162">The services will create a new dependency in the controller constructor, which will be resolved using Dependency Injection with the help of **Unity**.</span></span>

<span data-ttu-id="cce2f-163">Takie podejście pokazuje, jak generować mniej połączone aplikacje, które są bardziej elastyczne i łatwiejsze do utrzymania i testowania.</span><span class="sxs-lookup"><span data-stu-id="cce2f-163">This approach will show you how to generate less coupled applications, which are more flexible and easier to maintain and test.</span></span> <span data-ttu-id="cce2f-164">Dowiesz się również, jak zintegrować ASP.NET MVC z programem Unity.</span><span class="sxs-lookup"><span data-stu-id="cce2f-164">You will also learn how to integrate ASP.NET MVC with Unity.</span></span>

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a><span data-ttu-id="cce2f-165">Usługa Storemanager — informacje</span><span class="sxs-lookup"><span data-stu-id="cce2f-165">About StoreManager Service</span></span>

<span data-ttu-id="cce2f-166">Magazyn MVC Music udostępniony w rozwiązaniu BEGIN obejmuje teraz usługę, która zarządza danymi kontrolera magazynu o nazwie **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="cce2f-166">The MVC Music Store provided in the begin solution now includes a service that manages the Store Controller data named **StoreService**.</span></span> <span data-ttu-id="cce2f-167">Poniżej znajdziesz implementację usługi magazynu.</span><span class="sxs-lookup"><span data-stu-id="cce2f-167">Below you will find the Store Service implementation.</span></span> <span data-ttu-id="cce2f-168">Zwróć uwagę, że wszystkie metody zwracają jednostki modelu.</span><span class="sxs-lookup"><span data-stu-id="cce2f-168">Note that all the methods return Model entities.</span></span>

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

<span data-ttu-id="cce2f-169">**StoreController** z rozwiązania BEGIN korzysta teraz z **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="cce2f-169">**StoreController** from the begin solution now consumes **StoreService**.</span></span> <span data-ttu-id="cce2f-170">Wszystkie odwołania do danych zostały usunięte z **StoreController**i teraz możliwe jest zmodyfikowanie bieżącego dostawcy dostępu do danych bez zmiany żadnej metody korzystającej z **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="cce2f-170">All the data references were removed from **StoreController**, and now possible to modify the current data access provider without changing any method that consumes **StoreService**.</span></span>

<span data-ttu-id="cce2f-171">Poniżej znajdują się informacje o tym, że implementacja **StoreController** ma zależność z **StoreService** wewnątrz konstruktora klasy.</span><span class="sxs-lookup"><span data-stu-id="cce2f-171">You will find below that the **StoreController** implementation has a dependency with **StoreService** inside the class constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="cce2f-172">Zależność wprowadzona w tym ćwiczeniu jest związana z **niewersją kontroli** (IOC).</span><span class="sxs-lookup"><span data-stu-id="cce2f-172">The dependency introduced in this exercise is related to **Inversion of Control** (IoC).</span></span>
> 
> <span data-ttu-id="cce2f-173">Konstruktor klasy **StoreController** odbiera parametr typu **IStoreService** , który jest niezbędny do wykonywania wywołań usługi z wewnątrz klasy.</span><span class="sxs-lookup"><span data-stu-id="cce2f-173">The **StoreController** class constructor receives an **IStoreService** type parameter, which is essential to perform service calls from inside the class.</span></span> <span data-ttu-id="cce2f-174">Jednak **StoreController** nie implementuje domyślnego konstruktora (bez parametrów), które każdy kontroler musi mieć do pracy z ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="cce2f-174">However, **StoreController** does not implement the default constructor (with no parameters) that any controller must have to work with ASP.NET MVC.</span></span>
> 
> <span data-ttu-id="cce2f-175">Aby rozwiązać zależność, kontroler musi być utworzony przez fabrykę abstrakcyjną (Klasa, która zwraca dowolny obiekt określonego typu).</span><span class="sxs-lookup"><span data-stu-id="cce2f-175">To resolve the dependency, the controller has to be created by an abstract factory (a class that returns any object of the specified type).</span></span>

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="cce2f-176">Jeśli Klasa podejmie próbę utworzenia StoreController bez wysłania obiektu usługi, wystąpi błąd, ponieważ nie ma zadeklarowanego konstruktora bezparametrowego.</span><span class="sxs-lookup"><span data-stu-id="cce2f-176">You will get an error when the class tries to create the StoreController without sending the service object, as there is no parameterless constructor declared.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a><span data-ttu-id="cce2f-177">Zadanie 1 — Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="cce2f-177">Task 1 - Running the Application</span></span>

<span data-ttu-id="cce2f-178">W tym zadaniu uruchomisz aplikację BEGIN, która obejmuje usługę w kontrolerze magazynu, która oddziela dostęp do danych z logiki aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cce2f-178">In this task, you will run the Begin application, which includes the service into the Store Controller that separates the data access from the application logic.</span></span>

<span data-ttu-id="cce2f-179">Podczas uruchamiania aplikacji zostanie wyświetlony wyjątek, ponieważ usługa kontrolera nie zostanie domyślnie przeniesiona jako parametr:</span><span class="sxs-lookup"><span data-stu-id="cce2f-179">When running the application, you will receive an exception, as the controller service is not passed as a parameter by default:</span></span>

1. <span data-ttu-id="cce2f-180">Otwórz rozwiązanie **BEGIN** znajdujące się w **Source\Ex01-Injecting Controller\Begin**.</span><span class="sxs-lookup"><span data-stu-id="cce2f-180">Open the **Begin** solution located in **Source\Ex01-Injecting Controller\Begin**.</span></span>

   1. <span data-ttu-id="cce2f-181">Przed kontynuowaniem musisz pobrać brakujące pakiety NuGet.</span><span class="sxs-lookup"><span data-stu-id="cce2f-181">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="cce2f-182">W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="cce2f-182">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="cce2f-183">W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.</span><span class="sxs-lookup"><span data-stu-id="cce2f-183">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="cce2f-184">Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="cce2f-184">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="cce2f-185">Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="cce2f-185">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="cce2f-186">Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu.</span><span class="sxs-lookup"><span data-stu-id="cce2f-186">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="cce2f-187">To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="cce2f-187">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="cce2f-188">Naciśnij **klawisze CTRL + F5** , aby uruchomić aplikację bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="cce2f-188">Press **Ctrl + F5** to run the application without debugging.</span></span> <span data-ttu-id="cce2f-189">Zostanie wyświetlony komunikat o błędzie &quot;**dla tego obiektu nie zdefiniowano konstruktora bez parametrów**&quot;:</span><span class="sxs-lookup"><span data-stu-id="cce2f-189">You will get the error message &quot;**No parameterless constructor defined for this object**&quot;:</span></span>

    <span data-ttu-id="cce2f-190">![Wystąpił błąd podczas uruchamiania aplikacji ASP.NET MVC BEGIN](aspnet-mvc-4-dependency-injection/_static/image3.png "Wystąpił błąd podczas uruchamiania aplikacji ASP.NET MVC BEGIN")</span><span class="sxs-lookup"><span data-stu-id="cce2f-190">![Error while running ASP.NET MVC Begin Application](aspnet-mvc-4-dependency-injection/_static/image3.png "Error while running ASP.NET MVC Begin Application")</span></span>

    <span data-ttu-id="cce2f-191">*Wystąpił błąd podczas uruchamiania aplikacji ASP.NET MVC BEGIN*</span><span class="sxs-lookup"><span data-stu-id="cce2f-191">*Error while running ASP.NET MVC Begin Application*</span></span>
3. <span data-ttu-id="cce2f-192">Zamknij okno przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="cce2f-192">Close the browser.</span></span>

<span data-ttu-id="cce2f-193">W poniższych krokach zostanie wykonane rozwiązanie dotyczące magazynu utworów muzycznych, aby wstrzyknąć zależność wymaganą przez ten kontroler.</span><span class="sxs-lookup"><span data-stu-id="cce2f-193">In the following steps you will work on the Music Store Solution to inject the dependency this controller needs.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a><span data-ttu-id="cce2f-194">Zadanie 2 — w tym środowisko Unity w rozwiązaniu MvcMusicStore</span><span class="sxs-lookup"><span data-stu-id="cce2f-194">Task 2 - Including Unity into MvcMusicStore Solution</span></span>

<span data-ttu-id="cce2f-195">W tym zadaniu zostanie uwzględniony pakiet NuGet **Unity. Mvc3** do rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="cce2f-195">In this task, you will include **Unity.Mvc3** NuGet Package to the solution.</span></span>

> [!NOTE]
> <span data-ttu-id="cce2f-196">Pakiet Unity. Mvc3 został zaprojektowany dla ASP.NET MVC 3, ale jest w pełni zgodny z ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="cce2f-196">Unity.Mvc3 package was designed for ASP.NET MVC 3, but it is fully compatible with ASP.NET MVC 4.</span></span>
> 
> <span data-ttu-id="cce2f-197">Unity jest lekkim, rozszerzalnym kontenerem iniekcji zależności z opcjonalną obsługą przechwycenia wystąpienia i typu.</span><span class="sxs-lookup"><span data-stu-id="cce2f-197">Unity is a lightweight, extensible dependency injection container with optional support for instance and type interception.</span></span> <span data-ttu-id="cce2f-198">Jest to kontener ogólnego przeznaczenia do użycia w dowolnym typie aplikacji .NET.</span><span class="sxs-lookup"><span data-stu-id="cce2f-198">It is a general-purpose container for use in any type of .NET application.</span></span> <span data-ttu-id="cce2f-199">Zapewnia ona wszystkie typowe funkcje dostępne w mechanizmie iniekcji zależności, w tym: Tworzenie obiektów, abstrakcję wymagań przez określenie zależności w czasie wykonywania i elastyczność, przez odroczenie konfiguracji składnika do kontenera.</span><span class="sxs-lookup"><span data-stu-id="cce2f-199">It provides all the common features found in dependency injection mechanisms including: object creation, abstraction of requirements by specifying dependencies at runtime and flexibility, by deferring the component configuration to the container.</span></span>

1. <span data-ttu-id="cce2f-200">Zainstaluj pakiet NuGet **Unity. Mvc3** w projekcie **MvcMusicStore** .</span><span class="sxs-lookup"><span data-stu-id="cce2f-200">Install **Unity.Mvc3** NuGet Package in the **MvcMusicStore** project.</span></span> <span data-ttu-id="cce2f-201">W tym celu Otwórz **konsolę Menedżera pakietów** z **widoku** | **inne okna**.</span><span class="sxs-lookup"><span data-stu-id="cce2f-201">To do this, open the **Package Manager Console** from **View** | **Other Windows**.</span></span>
2. <span data-ttu-id="cce2f-202">Uruchom następujące polecenie.</span><span class="sxs-lookup"><span data-stu-id="cce2f-202">Run the following command.</span></span>

    <span data-ttu-id="cce2f-203">PMC</span><span class="sxs-lookup"><span data-stu-id="cce2f-203">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    <span data-ttu-id="cce2f-204">![Instalowanie pakietu NuGet aparatu Unity. Mvc3](aspnet-mvc-4-dependency-injection/_static/image4.png "Instalowanie pakietu NuGet aparatu Unity. Mvc3")</span><span class="sxs-lookup"><span data-stu-id="cce2f-204">![Installing Unity.Mvc3 NuGet Package](aspnet-mvc-4-dependency-injection/_static/image4.png "Installing Unity.Mvc3 NuGet Package")</span></span>

    <span data-ttu-id="cce2f-205">*Instalowanie pakietu NuGet aparatu Unity. Mvc3*</span><span class="sxs-lookup"><span data-stu-id="cce2f-205">*Installing Unity.Mvc3 NuGet Package*</span></span>
3. <span data-ttu-id="cce2f-206">Po zainstalowaniu pakietu **Unity. Mvc3** Eksploruj pliki i foldery, które są automatycznie dodawane, aby uprościć konfigurację środowiska Unity.</span><span class="sxs-lookup"><span data-stu-id="cce2f-206">Once the **Unity.Mvc3** package is installed, explore the files and folders it automatically adds in order to simplify Unity configuration.</span></span>

    <span data-ttu-id="cce2f-207">![Zainstalowano pakiet Unity. Mvc3](aspnet-mvc-4-dependency-injection/_static/image5.png "Zainstalowano pakiet Unity. Mvc3")</span><span class="sxs-lookup"><span data-stu-id="cce2f-207">![Unity.Mvc3 package installed](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 package installed")</span></span>

    <span data-ttu-id="cce2f-208">*Zainstalowano pakiet Unity. Mvc3*</span><span class="sxs-lookup"><span data-stu-id="cce2f-208">*Unity.Mvc3 package installed*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-application_start"></a><span data-ttu-id="cce2f-209">Zadanie 3 — rejestrowanie aparatu Unity w aplikacji Global.asax.cs\_Start</span><span class="sxs-lookup"><span data-stu-id="cce2f-209">Task 3 - Registering Unity in Global.asax.cs Application\_Start</span></span>

<span data-ttu-id="cce2f-210">W tym zadaniu zaktualizujesz **aplikację\_startową** znajdującą się w **Global.asax.cs** w celu wywołania inicjatora programu inicjującego środowisko uruchomieniowe Unity, a następnie zaktualizujesz plik programu inicjującego, który zostanie użyty do iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="cce2f-210">In this task, you will update the **Application\_Start** method located in **Global.asax.cs** to call the Unity Bootstrapper initializer and then, update the Bootstrapper file registering the Service and Controller you will use for Dependency Injection.</span></span>

1. <span data-ttu-id="cce2f-211">Teraz nastąpi podłączenie do programu inicjującego, który jest plikiem inicjującym kontener aparatu Unity i program rozpoznawania zależności.</span><span class="sxs-lookup"><span data-stu-id="cce2f-211">Now, you will hook up the Bootstrapper which is the file that initializes the Unity container and Dependency Resolver.</span></span> <span data-ttu-id="cce2f-212">Aby to zrobić, Otwórz **Global.asax.cs** i Dodaj następujący wyróżniony kod w **aplikacji\_Start** .</span><span class="sxs-lookup"><span data-stu-id="cce2f-212">To do this, open **Global.asax.cs** and add the following highlighted code within the **Application\_Start** method.</span></span>

    <span data-ttu-id="cce2f-213">(Fragment kodu — *ASP.net-test wtrysku zależności-Ex01-Initialize Unity*)</span><span class="sxs-lookup"><span data-stu-id="cce2f-213">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Initialize Unity*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. <span data-ttu-id="cce2f-214">Otwórz plik **Bootstrapper.cs** .</span><span class="sxs-lookup"><span data-stu-id="cce2f-214">Open **Bootstrapper.cs** file.</span></span>
3. <span data-ttu-id="cce2f-215">Uwzględnij następujące przestrzenie nazw: **MvcMusicStore. Services** i **MusicStore. controllers**.</span><span class="sxs-lookup"><span data-stu-id="cce2f-215">Include the following namespaces: **MvcMusicStore.Services** and **MusicStore.Controllers**.</span></span>

    <span data-ttu-id="cce2f-216">(Fragment kodu — *ASP.NET wtrysk zależności Lab-Ex01-program inicjujący Dodawanie przestrzeni nazw*)</span><span class="sxs-lookup"><span data-stu-id="cce2f-216">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. <span data-ttu-id="cce2f-217">Zastąp zawartość metody **BuildUnityContainer** następującym kodem, który rejestruje kontroler magazynu i usługę magazynu.</span><span class="sxs-lookup"><span data-stu-id="cce2f-217">Replace **BuildUnityContainer** method's content with the following code that registers Store Controller and Store Service.</span></span>

    <span data-ttu-id="cce2f-218">(Fragment kodu — *ASP.neta wtrysku zależności Ex01-Register — kontroler i usługa magazynu*)</span><span class="sxs-lookup"><span data-stu-id="cce2f-218">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Register Store Controller and Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="cce2f-219">Zadanie 4 — Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="cce2f-219">Task 4 - Running the Application</span></span>

<span data-ttu-id="cce2f-220">W tym zadaniu uruchomisz aplikację w celu sprawdzenia, czy można ją teraz załadować po dołączeniu aparatu Unity.</span><span class="sxs-lookup"><span data-stu-id="cce2f-220">In this task, you will run the application to verify that it can now be loaded after including Unity.</span></span>

1. <span data-ttu-id="cce2f-221">Naciśnij klawisz **F5** , aby uruchomić aplikację, aplikacja powinna teraz ładować bez wyświetlania komunikatu o błędzie.</span><span class="sxs-lookup"><span data-stu-id="cce2f-221">Press **F5** to run the application, the application should now load without showing any error message.</span></span>

    <span data-ttu-id="cce2f-222">![Uruchamianie aplikacji z iniekcją zależności](aspnet-mvc-4-dependency-injection/_static/image6.png "Uruchamianie aplikacji z iniekcją zależności")</span><span class="sxs-lookup"><span data-stu-id="cce2f-222">![Running Application with Dependency Injection](aspnet-mvc-4-dependency-injection/_static/image6.png "Running Application with Dependency Injection")</span></span>

    <span data-ttu-id="cce2f-223">*Uruchamianie aplikacji z iniekcją zależności*</span><span class="sxs-lookup"><span data-stu-id="cce2f-223">*Running Application with Dependency Injection*</span></span>
2. <span data-ttu-id="cce2f-224">Przejdź do **/Store**.</span><span class="sxs-lookup"><span data-stu-id="cce2f-224">Browse to **/Store**.</span></span> <span data-ttu-id="cce2f-225">Spowoduje to wywołanie **StoreController**, który jest teraz tworzony przy użyciu **aparatu Unity**.</span><span class="sxs-lookup"><span data-stu-id="cce2f-225">This will invoke **StoreController**, which is now created using **Unity**.</span></span>

    <span data-ttu-id="cce2f-226">![Sklep MVC Music](aspnet-mvc-4-dependency-injection/_static/image7.png "Sklep MVC Music")</span><span class="sxs-lookup"><span data-stu-id="cce2f-226">![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")</span></span>

    <span data-ttu-id="cce2f-227">*Sklep MVC Music*</span><span class="sxs-lookup"><span data-stu-id="cce2f-227">*MVC Music Store*</span></span>
3. <span data-ttu-id="cce2f-228">Zamknij okno przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="cce2f-228">Close the browser.</span></span>

<span data-ttu-id="cce2f-229">W poniższych ćwiczeniach dowiesz się, jak zwiększyć zakres iniekcji zależności, aby używać go w widokach ASP.NET MVC i filtrach akcji.</span><span class="sxs-lookup"><span data-stu-id="cce2f-229">In the following exercises you will learn how to extend the Dependency Injection scope to use it inside ASP.NET MVC Views and Action Filters.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a><span data-ttu-id="cce2f-230">Ćwiczenie 2: wprowadzanie widoku</span><span class="sxs-lookup"><span data-stu-id="cce2f-230">Exercise 2: Injecting a View</span></span>

<span data-ttu-id="cce2f-231">W tym ćwiczeniu dowiesz się, jak używać iniekcji zależności w widoku z nowymi funkcjami integracji ASP.NET MVC 4 for Unity.</span><span class="sxs-lookup"><span data-stu-id="cce2f-231">In this exercise, you will learn how to use Dependency Injection in a view with the new features of ASP.NET MVC 4 for Unity integration.</span></span> <span data-ttu-id="cce2f-232">W tym celu w widoku przeglądania sklepu zostanie wywołana Usługa niestandardowa, w której zostanie wyświetlony komunikat i obraz poniżej.</span><span class="sxs-lookup"><span data-stu-id="cce2f-232">In order to do that, you will call a custom service inside the Store Browse View, which will show a message and an image below.</span></span>

<span data-ttu-id="cce2f-233">Następnie można zintegrować projekt z aparatem Unity i utworzyć niestandardowy program rozpoznawania zależności, aby wstrzyknąć zależności.</span><span class="sxs-lookup"><span data-stu-id="cce2f-233">Then, you will integrate the project with Unity and create a custom dependency resolver to inject the dependencies.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a><span data-ttu-id="cce2f-234">Zadanie 1 — Tworzenie widoku korzystającego z usługi</span><span class="sxs-lookup"><span data-stu-id="cce2f-234">Task 1 - Creating a View that Consumes a Service</span></span>

<span data-ttu-id="cce2f-235">W tym zadaniu utworzysz widok, który wykonuje wywołanie usługi, aby wygenerować nową zależność.</span><span class="sxs-lookup"><span data-stu-id="cce2f-235">In this task, you will create a view that performs a service call to generate a new dependency.</span></span> <span data-ttu-id="cce2f-236">Usługa programu składa się z prostej usługi obsługi komunikatów zawartej w tym rozwiązaniu.</span><span class="sxs-lookup"><span data-stu-id="cce2f-236">The service consists in a simple messaging service included in this solution.</span></span>

1. <span data-ttu-id="cce2f-237">Otwórz rozwiązanie **BEGIN** znajdujące się w folderze **Source\Ex02-Injecting View\Begin** .</span><span class="sxs-lookup"><span data-stu-id="cce2f-237">Open the **Begin** solution located in the **Source\Ex02-Injecting View\Begin** folder.</span></span> <span data-ttu-id="cce2f-238">W przeciwnym razie można nadal korzystać z rozwiązania **końcowego** uzyskanego przez zakończenie poprzedniego ćwiczenia.</span><span class="sxs-lookup"><span data-stu-id="cce2f-238">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="cce2f-239">Jeśli otwarto **udostępnione rozwiązanie** , należy pobrać brakujące pakiety NuGet przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="cce2f-239">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="cce2f-240">W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="cce2f-240">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="cce2f-241">W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.</span><span class="sxs-lookup"><span data-stu-id="cce2f-241">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="cce2f-242">Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="cce2f-242">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="cce2f-243">Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="cce2f-243">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="cce2f-244">Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu.</span><span class="sxs-lookup"><span data-stu-id="cce2f-244">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="cce2f-245">To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="cce2f-245">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="cce2f-246">Aby uzyskać więcej informacji, zobacz ten artykuł: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="cce2f-246">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="cce2f-247">Uwzględnij klasy **MessageService.cs** i **IMessageService.cs** znajdujące się w folderze **Source \Assets** w **/Services**.</span><span class="sxs-lookup"><span data-stu-id="cce2f-247">Include the **MessageService.cs** and the **IMessageService.cs** classes located in the **Source \Assets** folder in **/Services**.</span></span> <span data-ttu-id="cce2f-248">Aby to zrobić, kliknij prawym przyciskiem myszy folder **usługi** i wybierz pozycję **Dodaj istniejący element**.</span><span class="sxs-lookup"><span data-stu-id="cce2f-248">To do this, right-click **Services** folder and select **Add Existing Item**.</span></span> <span data-ttu-id="cce2f-249">Przejdź do lokalizacji plików i Uwzględnij je.</span><span class="sxs-lookup"><span data-stu-id="cce2f-249">Browse to the files' location and include them.</span></span>

    <span data-ttu-id="cce2f-250">![Dodawanie usługi komunikatów i interfejsu usługi](aspnet-mvc-4-dependency-injection/_static/image8.png "Dodawanie usługi komunikatów i interfejsu usługi")</span><span class="sxs-lookup"><span data-stu-id="cce2f-250">![Adding Message Service and Service Interface](aspnet-mvc-4-dependency-injection/_static/image8.png "Adding Message Service and Service Interface")</span></span>

    <span data-ttu-id="cce2f-251">*Dodawanie usługi komunikatów i interfejsu usługi*</span><span class="sxs-lookup"><span data-stu-id="cce2f-251">*Adding Message Service and Service Interface*</span></span>

    > [!NOTE]
    > <span data-ttu-id="cce2f-252">Interfejs **IMessageService** definiuje dwie właściwości zaimplementowane przez klasę **MessageService** .</span><span class="sxs-lookup"><span data-stu-id="cce2f-252">The **IMessageService** interface defines two properties implemented by the **MessageService** class.</span></span> <span data-ttu-id="cce2f-253">Te właściwości —**Message** i **ImageUrl**— PRZECHOWUJĄ komunikat i adres URL obrazu, który będzie wyświetlany.</span><span class="sxs-lookup"><span data-stu-id="cce2f-253">These properties -**Message** and **ImageUrl**- store the message and the URL of the image to be displayed.</span></span>
3. <span data-ttu-id="cce2f-254">Utwórz folder **/Pages** w folderze głównym projektu, a następnie Dodaj istniejącą klasę **MyBasePage.cs** z **Source\Assets**.</span><span class="sxs-lookup"><span data-stu-id="cce2f-254">Create the folder **/Pages** in the project's root folder, and then add the existing class **MyBasePage.cs** from **Source\Assets**.</span></span> <span data-ttu-id="cce2f-255">Strona podstawowa, z której będzie dziedziczyć, ma następującą strukturę.</span><span class="sxs-lookup"><span data-stu-id="cce2f-255">The base page you will inherit from has the following structure.</span></span>

    <span data-ttu-id="cce2f-256">![Folder stron](aspnet-mvc-4-dependency-injection/_static/image9.png "Folder stron")</span><span class="sxs-lookup"><span data-stu-id="cce2f-256">![Pages folder](aspnet-mvc-4-dependency-injection/_static/image9.png "Pages folder")</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. <span data-ttu-id="cce2f-257">Otwórz widok **Przeglądaj. cshtml** z folderu **/views/Store** i uczyń go dziedziczonym z **MyBasePage.cs**.</span><span class="sxs-lookup"><span data-stu-id="cce2f-257">Open **Browse.cshtml** view from **/Views/Store** folder, and make it inherit from **MyBasePage.cs**.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. <span data-ttu-id="cce2f-258">W widoku **przeglądania** Dodaj wywołanie do **MessageService** w celu wyświetlenia obrazu i wiadomości pobranej przez usługę.</span><span class="sxs-lookup"><span data-stu-id="cce2f-258">In the **Browse** view, add a call to **MessageService** to display an image and a message retrieved by the service.</span></span>
   <span data-ttu-id="cce2f-259">(C#)</span><span class="sxs-lookup"><span data-stu-id="cce2f-259">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a><span data-ttu-id="cce2f-260">Zadanie 2 — łącznie z niestandardowym programem rozpoznawania zależności i niestandardowym aktywatorem strony widoku</span><span class="sxs-lookup"><span data-stu-id="cce2f-260">Task 2 - Including a Custom Dependency Resolver and a Custom View Page Activator</span></span>

<span data-ttu-id="cce2f-261">W poprzednim zadaniu została wprowadzona nowa zależność wewnątrz widoku w celu wykonania w nim wywołania usługi.</span><span class="sxs-lookup"><span data-stu-id="cce2f-261">In the previous task, you injected a new dependency inside a view to perform a service call inside it.</span></span> <span data-ttu-id="cce2f-262">Teraz można rozwiązać ten zależność przez implementację interfejsów wstrzykiwania zależności ASP.NET MVC **IViewPageActivator** i **IDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="cce2f-262">Now, you will resolve that dependency by implementing the ASP.NET MVC Dependency Injection interfaces **IViewPageActivator** and **IDependencyResolver**.</span></span> <span data-ttu-id="cce2f-263">W rozwiązaniu zostaną uwzględnione implementacje **IDependencyResolver** , które będą obsługiwać pobieranie usługi przy użyciu aparatu Unity.</span><span class="sxs-lookup"><span data-stu-id="cce2f-263">You will include in the solution an implementation of **IDependencyResolver** that will deal with the service retrieval by using Unity.</span></span> <span data-ttu-id="cce2f-264">Następnie dołączysz kolejną niestandardową implementację interfejsu **IViewPageActivator** , która będzie rozwiązywać Tworzenie widoków.</span><span class="sxs-lookup"><span data-stu-id="cce2f-264">Then, you will include another custom implementation of **IViewPageActivator** interface that will solve the creation of the views.</span></span>

> [!NOTE]
> <span data-ttu-id="cce2f-265">Ponieważ ASP.NET MVC 3, implementacja dla iniekcji zależności miała Uproszczone interfejsy do rejestracji usług.</span><span class="sxs-lookup"><span data-stu-id="cce2f-265">Since ASP.NET MVC 3, the implementation for Dependency Injection had simplified the interfaces to register services.</span></span> <span data-ttu-id="cce2f-266">**IDependencyResolver** i **IViewPageActivator** są częścią funkcji ASP.NET MVC 3 dla iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="cce2f-266">**IDependencyResolver** and **IViewPageActivator** are part of ASP.NET MVC 3 features for Dependency Injection.</span></span>
> 
> <span data-ttu-id="cce2f-267">**-IDependencyResolver** interfejs zastępuje poprzednią IMvcServiceLocator.</span><span class="sxs-lookup"><span data-stu-id="cce2f-267">**- IDependencyResolver** interface replaces the previous IMvcServiceLocator.</span></span> <span data-ttu-id="cce2f-268">Implementacje elementu IDependencyResolver muszą zwracać wystąpienie usługi lub kolekcji usług.</span><span class="sxs-lookup"><span data-stu-id="cce2f-268">Implementers of IDependencyResolver must return an instance of the service or a service collection.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> <span data-ttu-id="cce2f-269">**-IViewPageActivator** interfejs zapewnia bardziej precyzyjną kontrolę nad sposobem tworzenia wystąpień stron widoku przy użyciu iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="cce2f-269">**- IViewPageActivator** interface provides more fine-grained control over how view pages are instantiated via dependency injection.</span></span> <span data-ttu-id="cce2f-270">Klasy, które implementują interfejs **IViewPageActivator** , mogą tworzyć wystąpienia widoku przy użyciu informacji kontekstowych.</span><span class="sxs-lookup"><span data-stu-id="cce2f-270">The classes that implement **IViewPageActivator** interface can create view instances using context information.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]

1. <span data-ttu-id="cce2f-271">Utwórz folder**fabryks** w folderze głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="cce2f-271">Create the /**Factories** folder in the project's root folder.</span></span>
2. <span data-ttu-id="cce2f-272">Uwzględnij **CustomViewPageActivator.cs** do rozwiązania z folderu **/sources/Assets/** do **Factors** .</span><span class="sxs-lookup"><span data-stu-id="cce2f-272">Include **CustomViewPageActivator.cs** to your solution from **/Sources/Assets/** to **Factories** folder.</span></span> <span data-ttu-id="cce2f-273">Aby to zrobić, kliknij prawym przyciskiem myszy folder **/Factories** , a następnie wybierz pozycję **Dodaj | Istniejący element** , a następnie wybierz pozycję **CustomViewPageActivator.cs**.</span><span class="sxs-lookup"><span data-stu-id="cce2f-273">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **CustomViewPageActivator.cs**.</span></span> <span data-ttu-id="cce2f-274">Ta klasa implementuje interfejs **IViewPageActivator** do przechowywania kontenera Unity.</span><span class="sxs-lookup"><span data-stu-id="cce2f-274">This class implements the **IViewPageActivator** interface to hold the Unity Container.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > <span data-ttu-id="cce2f-275">**CustomViewPageActivator** jest odpowiedzialny za zarządzanie tworzeniem widoku przy użyciu kontenera Unity.</span><span class="sxs-lookup"><span data-stu-id="cce2f-275">**CustomViewPageActivator** is responsible for managing the creation of a view by using a Unity container.</span></span>
3. <span data-ttu-id="cce2f-276">Dołącz plik **UnityDependencyResolver.cs** z folderu **/sources/Assets** do **/Factories** .</span><span class="sxs-lookup"><span data-stu-id="cce2f-276">Include **UnityDependencyResolver.cs** file from **/Sources/Assets** to **/Factories** folder.</span></span> <span data-ttu-id="cce2f-277">Aby to zrobić, kliknij prawym przyciskiem myszy folder **/Factories** , a następnie wybierz pozycję **Dodaj | Istniejący element** , a następnie wybierz plik **UnityDependencyResolver.cs** .</span><span class="sxs-lookup"><span data-stu-id="cce2f-277">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **UnityDependencyResolver.cs** file.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > <span data-ttu-id="cce2f-278">Klasa **UnityDependencyResolver** jest niestandardowym DependencyResolver dla aparatu Unity.</span><span class="sxs-lookup"><span data-stu-id="cce2f-278">**UnityDependencyResolver** class is a custom DependencyResolver for Unity.</span></span> <span data-ttu-id="cce2f-279">Gdy nie można znaleźć usługi wewnątrz kontenera Unity, podstawowy program rozpoznawania nazw to invocated.</span><span class="sxs-lookup"><span data-stu-id="cce2f-279">When a service cannot be found inside the Unity container, the base resolver is invocated.</span></span>

<span data-ttu-id="cce2f-280">W następujących zadaniach obie implementacje zostaną zarejestrowane w celu umożliwienia modelowi znajomości lokalizacji usług i widoków.</span><span class="sxs-lookup"><span data-stu-id="cce2f-280">In the following task both implementations will be registered to let the model know the location of the services and the views.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a><span data-ttu-id="cce2f-281">Zadanie 3 — rejestrowanie dla iniekcji zależności w kontenerze Unity</span><span class="sxs-lookup"><span data-stu-id="cce2f-281">Task 3 - Registering for Dependency Injection within Unity container</span></span>

<span data-ttu-id="cce2f-282">W tym zadaniu zostaną umieszczone wszystkie poprzednie elementy, aby umożliwić iniekcję zależności.</span><span class="sxs-lookup"><span data-stu-id="cce2f-282">In this task, you will put all the previous things together to make Dependency Injection work.</span></span>

<span data-ttu-id="cce2f-283">Do teraz Twoje rozwiązanie ma następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="cce2f-283">Up to now your solution has the following elements:</span></span>

- <span data-ttu-id="cce2f-284">Widok **przeglądania** , który dziedziczy z **MyBaseClass** i korzysta z **MessageService**.</span><span class="sxs-lookup"><span data-stu-id="cce2f-284">A **Browse** View that inherits from **MyBaseClass** and consumes **MessageService**.</span></span>
- <span data-ttu-id="cce2f-285">Pośrednia Klasa-**MyBaseClass**-, która ma iniekcję zależności zadeklarowaną dla interfejsu usługi.</span><span class="sxs-lookup"><span data-stu-id="cce2f-285">An intermediate class -**MyBaseClass**- that has dependency injection declared for the service interface.</span></span>
- <span data-ttu-id="cce2f-286">Usługa- **MessageService** -i jej interfejs **IMessageService**.</span><span class="sxs-lookup"><span data-stu-id="cce2f-286">A service - **MessageService** - and its interface **IMessageService**.</span></span>
- <span data-ttu-id="cce2f-287">Niestandardowy program rozpoznawania zależności dla aparatu Unity- **UnityDependencyResolver** — który zajmuje się pobieraniem usługi.</span><span class="sxs-lookup"><span data-stu-id="cce2f-287">A custom dependency resolver for Unity - **UnityDependencyResolver** - that deals with service retrieval.</span></span>
- <span data-ttu-id="cce2f-288">Aktywator strony widoku — **CustomViewPageActivator** — tworzy stronę.</span><span class="sxs-lookup"><span data-stu-id="cce2f-288">A View Page activator - **CustomViewPageActivator** - that creates the page.</span></span>

<span data-ttu-id="cce2f-289">Aby wstrzyknąć widok **przeglądania** , można teraz zarejestrować niestandardowy program rozpoznawania zależności w kontenerze aparatu Unity.</span><span class="sxs-lookup"><span data-stu-id="cce2f-289">To inject **Browse** View, you will now register the custom dependency resolver in the Unity container.</span></span>

1. <span data-ttu-id="cce2f-290">Otwórz plik **Bootstrapper.cs** .</span><span class="sxs-lookup"><span data-stu-id="cce2f-290">Open **Bootstrapper.cs** file.</span></span>
2. <span data-ttu-id="cce2f-291">Zarejestruj wystąpienie elementu **MessageService** w kontenerze Unity, aby zainicjować usługę:</span><span class="sxs-lookup"><span data-stu-id="cce2f-291">Register an instance of **MessageService** into the Unity container to initialize the service:</span></span>

    <span data-ttu-id="cce2f-292">(Fragment kodu — *ASP.NETe iniekcja zależności — Ex02 — rejestrowanie usługi komunikatów*)</span><span class="sxs-lookup"><span data-stu-id="cce2f-292">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register Message Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. <span data-ttu-id="cce2f-293">Dodaj odwołanie do przestrzeni nazw **MvcMusicStore. Factors** .</span><span class="sxs-lookup"><span data-stu-id="cce2f-293">Add a reference to **MvcMusicStore.Factories** namespace.</span></span>

    <span data-ttu-id="cce2f-294">(Fragment kodu — *ASP.NET wtrysk zależności Lab-Ex02-Factors*)</span><span class="sxs-lookup"><span data-stu-id="cce2f-294">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Factories Namespace*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. <span data-ttu-id="cce2f-295">Zarejestruj **CustomViewPageActivator** jako aktywator strony widoku w kontenerze Unity:</span><span class="sxs-lookup"><span data-stu-id="cce2f-295">Register **CustomViewPageActivator** as a View Page activator into the Unity container:</span></span>

    <span data-ttu-id="cce2f-296">(Fragment kodu — *ASP.NET wtrysku zależności-Ex02-Register CustomViewPageActivator*)</span><span class="sxs-lookup"><span data-stu-id="cce2f-296">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register CustomViewPageActivator*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. <span data-ttu-id="cce2f-297">Zamień domyślny program rozpoznawania zależności ASP.NET MVC 4 z wystąpieniem **UnityDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="cce2f-297">Replace ASP.NET MVC 4 default dependency resolver with an instance of **UnityDependencyResolver**.</span></span> <span data-ttu-id="cce2f-298">Aby to zrobić, Zastąp zawartość metody **Initialize** następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="cce2f-298">To do this, replace **Initialize** method content with the following code:</span></span>

    <span data-ttu-id="cce2f-299">(Fragment kodu — *ASP.NET-Ex02-test wtrysku zależności — Aktualizacja programu rozpoznawania zależności*)</span><span class="sxs-lookup"><span data-stu-id="cce2f-299">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Update Dependency Resolver*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > <span data-ttu-id="cce2f-300">ASP.NET MVC udostępnia domyślną klasę programu rozpoznawania zależności.</span><span class="sxs-lookup"><span data-stu-id="cce2f-300">ASP.NET MVC provides a default dependency resolver class.</span></span> <span data-ttu-id="cce2f-301">Aby można było korzystać z niestandardowych elementów rozpoznawania zależności jako tej, która została utworzona dla aparatu Unity, należy wymienić ten program rozpoznawania nazw.</span><span class="sxs-lookup"><span data-stu-id="cce2f-301">To work with custom dependency resolvers as the one we have created for unity, this resolver has to be replaced.</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="cce2f-302">Zadanie 4 — Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="cce2f-302">Task 4 - Running the Application</span></span>

<span data-ttu-id="cce2f-303">W tym zadaniu uruchomisz aplikację w celu sprawdzenia, czy przeglądarka sklepu korzysta z usługi i pokazuje obraz oraz pobraną wiadomość:</span><span class="sxs-lookup"><span data-stu-id="cce2f-303">In this task, you will run the application to verify that the Store Browser consumes the service and shows the image and the message retrieved:</span></span>

1. <span data-ttu-id="cce2f-304">Naciśnij klawisz **F5**, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="cce2f-304">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="cce2f-305">Kliknij pozycję **Rock** w menu gatuneks i zobacz, jak **MessageService** został wstrzykiwany do widoku i załadował Komunikat powitalny i obraz.</span><span class="sxs-lookup"><span data-stu-id="cce2f-305">Click **Rock** within the Genres Menu and see how the **MessageService** was injected to the view and loaded the welcome message and the image.</span></span> <span data-ttu-id="cce2f-306">W tym przykładzie wprowadzamy do &quot;**skalą**&quot;:</span><span class="sxs-lookup"><span data-stu-id="cce2f-306">In this example, we are entering to &quot;**Rock**&quot;:</span></span>

    <span data-ttu-id="cce2f-307">![Sklep MVC Music — iniekcja widoku](aspnet-mvc-4-dependency-injection/_static/image10.png "Sklep MVC Music — iniekcja widoku")</span><span class="sxs-lookup"><span data-stu-id="cce2f-307">![MVC Music Store - View Injection](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store - View Injection")</span></span>

    <span data-ttu-id="cce2f-308">*Sklep MVC Music — iniekcja widoku*</span><span class="sxs-lookup"><span data-stu-id="cce2f-308">*MVC Music Store - View Injection*</span></span>
3. <span data-ttu-id="cce2f-309">Zamknij okno przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="cce2f-309">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a><span data-ttu-id="cce2f-310">Ćwiczenie 3: wprowadzanie filtrów akcji</span><span class="sxs-lookup"><span data-stu-id="cce2f-310">Exercise 3: Injecting Action Filters</span></span>

<span data-ttu-id="cce2f-311">Zgodnie z poprzednimi **niestandardowymi akcjami** w laboratorium — filtry dostosowania i iniekcji zostały wprowadzone przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="cce2f-311">In the previous Hands-On lab **Custom Action Filters** you have worked with filters customization and injection.</span></span> <span data-ttu-id="cce2f-312">W tym ćwiczeniu dowiesz się, jak wstrzyknąć filtry z iniekcją zależności przy użyciu kontenera Unity.</span><span class="sxs-lookup"><span data-stu-id="cce2f-312">In this exercise, you will learn how to inject filters with Dependency Injection by using the Unity container.</span></span> <span data-ttu-id="cce2f-313">W tym celu należy dodać do rozwiązania do sklepu muzycznego filtr akcji niestandardowej, który będzie śledzić działanie lokacji.</span><span class="sxs-lookup"><span data-stu-id="cce2f-313">To do that, you will add to the Music Store solution a custom action filter that will trace the activity of the site.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a><span data-ttu-id="cce2f-314">Zadanie 1 — w tym filtr śledzenia w rozwiązaniu</span><span class="sxs-lookup"><span data-stu-id="cce2f-314">Task 1 - Including the Tracking Filter in the Solution</span></span>

<span data-ttu-id="cce2f-315">W tym zadaniu dojdziesz do funkcji filtrowania akcji niestandardowych w sklepie muzyka.</span><span class="sxs-lookup"><span data-stu-id="cce2f-315">In this task, you will include in the Music Store a custom action filter to trace events.</span></span> <span data-ttu-id="cce2f-316">Ponieważ koncepcje filtrów niestandardowych są już traktowane w poprzednim &quot;ych niestandardowych filtrach akcji&quot;, wystarczy dołączyć klasę Filter z folderu Assets tego laboratorium, a następnie utworzyć dostawcę filtrów dla aparatu Unity:</span><span class="sxs-lookup"><span data-stu-id="cce2f-316">As custom action filter concepts are already treated in the previous Lab &quot;Custom Action Filters&quot;, you will just include the filter class from the Assets folder of this lab, and then create a Filter Provider for Unity:</span></span>

1. <span data-ttu-id="cce2f-317">Otwórz rozwiązanie **BEGIN** znajdujące się w folderze **Source\Ex03-wstrzyknięcie akcji Filter\Begin** .</span><span class="sxs-lookup"><span data-stu-id="cce2f-317">Open the **Begin** solution located in the **Source\Ex03 - Injecting Action Filter\Begin** folder.</span></span> <span data-ttu-id="cce2f-318">W przeciwnym razie można nadal korzystać z rozwiązania **końcowego** uzyskanego przez zakończenie poprzedniego ćwiczenia.</span><span class="sxs-lookup"><span data-stu-id="cce2f-318">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="cce2f-319">Jeśli otwarto **udostępnione rozwiązanie** , należy pobrać brakujące pakiety NuGet przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="cce2f-319">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="cce2f-320">W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="cce2f-320">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="cce2f-321">W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.</span><span class="sxs-lookup"><span data-stu-id="cce2f-321">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="cce2f-322">Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="cce2f-322">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="cce2f-323">Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="cce2f-323">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="cce2f-324">Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu.</span><span class="sxs-lookup"><span data-stu-id="cce2f-324">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="cce2f-325">To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="cce2f-325">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="cce2f-326">Aby uzyskać więcej informacji, zobacz ten artykuł: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="cce2f-326">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="cce2f-327">Dołącz plik **TraceActionFilter.cs** z folderu **/sources/Assets** do **/filters** .</span><span class="sxs-lookup"><span data-stu-id="cce2f-327">Include **TraceActionFilter.cs** file from **/Sources/Assets** to **/Filters** folder.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > <span data-ttu-id="cce2f-328">Ten filtr akcji niestandardowej wykonuje śledzenie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="cce2f-328">This custom action filter performs ASP.NET tracing.</span></span> <span data-ttu-id="cce2f-329">Aby uzyskać więcej informacji, możesz sprawdzić, &quot;ASP.NET lokalny i dynamiczny filtrów akcji&quot; Lab.</span><span class="sxs-lookup"><span data-stu-id="cce2f-329">You can check &quot;ASP.NET MVC 4 local and Dynamic Action Filters&quot; Lab for more reference.</span></span>
3. <span data-ttu-id="cce2f-330">Dodaj pustą klasę **FilterProvider.cs** do projektu w folderze **/Filters.**</span><span class="sxs-lookup"><span data-stu-id="cce2f-330">Add the empty class **FilterProvider.cs** to the project in the folder **/Filters.**</span></span>
4. <span data-ttu-id="cce2f-331">Dodaj przestrzenie nazw **System. Web. MVC** i **Microsoft. Practices. Unity** w **FilterProvider.cs**.</span><span class="sxs-lookup"><span data-stu-id="cce2f-331">Add the **System.Web.Mvc** and **Microsoft.Practices.Unity** namespaces in **FilterProvider.cs**.</span></span>

    <span data-ttu-id="cce2f-332">(Fragment kodu — *ASP.NET wtrysk zależności Lab-Ex03-Filter Provider Dodawanie przestrzeni nazw*)</span><span class="sxs-lookup"><span data-stu-id="cce2f-332">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. <span data-ttu-id="cce2f-333">Ustaw klasę jako dziedziczą z interfejsu **IFilterProvider** .</span><span class="sxs-lookup"><span data-stu-id="cce2f-333">Make the class inherit from **IFilterProvider** Interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. <span data-ttu-id="cce2f-334">Dodaj właściwość **IUnityContainer** w klasie **FilterProvider** , a następnie Utwórz konstruktora klasy w celu przypisania kontenera.</span><span class="sxs-lookup"><span data-stu-id="cce2f-334">Add a **IUnityContainer** property in the **FilterProvider** class, and then create a class constructor to assign the container.</span></span>

    <span data-ttu-id="cce2f-335">(Fragment kodu — *ASP.NET wtrysk zależności Lab-Ex03 — Konstruktor dostawcy filtrów*)</span><span class="sxs-lookup"><span data-stu-id="cce2f-335">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Constructor*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > <span data-ttu-id="cce2f-336">Konstruktor klasy dostawcy filtrów nie tworzy **nowego** obiektu wewnątrz.</span><span class="sxs-lookup"><span data-stu-id="cce2f-336">The filter provider class constructor is not creating a **new** object inside.</span></span> <span data-ttu-id="cce2f-337">Kontener jest przekazaniem jako parametr, a zależność jest rozwiązany przez Unity.</span><span class="sxs-lookup"><span data-stu-id="cce2f-337">The container is passed as a parameter, and the dependency is solved by Unity.</span></span>
7. <span data-ttu-id="cce2f-338">W klasie **FilterProvider** Zaimplementuj metodę **getfilters** z interfejsu **IFilterProvider** .</span><span class="sxs-lookup"><span data-stu-id="cce2f-338">In the **FilterProvider** class, implement the method **GetFilters** from **IFilterProvider** interface.</span></span>

    <span data-ttu-id="cce2f-339">(Fragment kodu — *ASP.NET wtrysk zależności Lab-Ex03-Filter Provider Getfilters*)</span><span class="sxs-lookup"><span data-stu-id="cce2f-339">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider GetFilters*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a><span data-ttu-id="cce2f-340">Zadanie 2 — rejestrowanie i włączanie filtru</span><span class="sxs-lookup"><span data-stu-id="cce2f-340">Task 2 - Registering and Enabling the Filter</span></span>

<span data-ttu-id="cce2f-341">W tym zadaniu zostanie włączone śledzenie witryn.</span><span class="sxs-lookup"><span data-stu-id="cce2f-341">In this task, you will enable site tracking.</span></span> <span data-ttu-id="cce2f-342">W tym celu należy zarejestrować filtr w metodzie **Bootstrapper.cs BuildUnityContainer** , aby rozpocząć śledzenie:</span><span class="sxs-lookup"><span data-stu-id="cce2f-342">To do that, you will register the filter in **Bootstrapper.cs BuildUnityContainer** method to start tracing:</span></span>

1. <span data-ttu-id="cce2f-343">Otwórz **plik Web. config** znajdujący się w katalogu głównym projektu i Włącz śledzenie śledzenia w grupie system. Web.</span><span class="sxs-lookup"><span data-stu-id="cce2f-343">Open **Web.config** located in the project root and enable trace tracking at System.Web group.</span></span>

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. <span data-ttu-id="cce2f-344">Otwórz **Bootstrapper.cs** w katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="cce2f-344">Open **Bootstrapper.cs** at project root.</span></span>
3. <span data-ttu-id="cce2f-345">Dodaj odwołanie do przestrzeni nazw **MvcMusicStore. filters** .</span><span class="sxs-lookup"><span data-stu-id="cce2f-345">Add a reference to the **MvcMusicStore.Filters** namespace.</span></span>

    <span data-ttu-id="cce2f-346">(Fragment kodu — *ASP.NET wtrysk zależności Lab-Ex03-program inicjujący Dodawanie przestrzeni nazw*)</span><span class="sxs-lookup"><span data-stu-id="cce2f-346">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. <span data-ttu-id="cce2f-347">Wybierz metodę **BuildUnityContainer** i zarejestruj filtr w kontenerze Unity.</span><span class="sxs-lookup"><span data-stu-id="cce2f-347">Select the **BuildUnityContainer** method and register the filter in the Unity Container.</span></span> <span data-ttu-id="cce2f-348">Konieczne będzie zarejestrowanie dostawcy filtrów oraz filtru akcji.</span><span class="sxs-lookup"><span data-stu-id="cce2f-348">You will have to register the filter provider as well as the action filter.</span></span>

    <span data-ttu-id="cce2f-349">(Fragment kodu — *ASP.NET-Ex03-Register-FilterProvider i ActionFilter*)</span><span class="sxs-lookup"><span data-stu-id="cce2f-349">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Register FilterProvider and ActionFilter*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="cce2f-350">Zadanie 3 — Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="cce2f-350">Task 3 - Running the Application</span></span>

<span data-ttu-id="cce2f-351">W tym zadaniu uruchomisz aplikację i przetestujesz, że filtr akcji niestandardowej jest śledzony dla działania:</span><span class="sxs-lookup"><span data-stu-id="cce2f-351">In this task, you will run the application and test that the custom action filter is tracing the activity:</span></span>

1. <span data-ttu-id="cce2f-352">Naciśnij klawisz **F5**, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="cce2f-352">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="cce2f-353">Kliknij pozycję **Rock** w menu gatuneks.</span><span class="sxs-lookup"><span data-stu-id="cce2f-353">Click **Rock** within the Genres Menu.</span></span> <span data-ttu-id="cce2f-354">Jeśli chcesz, możesz przejść do kolejnych gatunków.</span><span class="sxs-lookup"><span data-stu-id="cce2f-354">You can browse to more genres if you want to.</span></span>

    <span data-ttu-id="cce2f-355">![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")</span><span class="sxs-lookup"><span data-stu-id="cce2f-355">![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")</span></span>

    <span data-ttu-id="cce2f-356">*Music Store*</span><span class="sxs-lookup"><span data-stu-id="cce2f-356">*Music Store*</span></span>
3. <span data-ttu-id="cce2f-357">Przejdź do **/Trace.axd** , aby wyświetlić stronę śledzenia aplikacji, a następnie kliknij pozycję **Wyświetl szczegóły**.</span><span class="sxs-lookup"><span data-stu-id="cce2f-357">Browse to **/Trace.axd** to see the Application Trace page, and then click **View Details**.</span></span>

    <span data-ttu-id="cce2f-358">![Dziennik śledzenia aplikacji](aspnet-mvc-4-dependency-injection/_static/image12.png "Dziennik śledzenia aplikacji")</span><span class="sxs-lookup"><span data-stu-id="cce2f-358">![Application Trace Log](aspnet-mvc-4-dependency-injection/_static/image12.png "Application Trace Log")</span></span>

    <span data-ttu-id="cce2f-359">*Dziennik śledzenia aplikacji*</span><span class="sxs-lookup"><span data-stu-id="cce2f-359">*Application Trace Log*</span></span>

    <span data-ttu-id="cce2f-360">![Śledzenie aplikacji — szczegóły żądania](aspnet-mvc-4-dependency-injection/_static/image13.png "Śledzenie aplikacji — szczegóły żądania")</span><span class="sxs-lookup"><span data-stu-id="cce2f-360">![Application Trace - Request Details](aspnet-mvc-4-dependency-injection/_static/image13.png "Application Trace - Request Details")</span></span>

    <span data-ttu-id="cce2f-361">*Śledzenie aplikacji — szczegóły żądania*</span><span class="sxs-lookup"><span data-stu-id="cce2f-361">*Application Trace - Request Details*</span></span>
4. <span data-ttu-id="cce2f-362">Zamknij okno przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="cce2f-362">Close the browser.</span></span>

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="cce2f-363">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="cce2f-363">Summary</span></span>

<span data-ttu-id="cce2f-364">Korzystając z tego praktycznego laboratorium, wiesz już, jak używać iniekcji zależności w ASP.NET MVC 4 przez integrację aparatu Unity przy użyciu pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="cce2f-364">By completing this Hands-On Lab you have learned how to use Dependency Injection in ASP.NET MVC 4 by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="cce2f-365">Aby to osiągnąć, można użyć iniekcji zależności wewnątrz kontrolerów, widoków i filtrów akcji.</span><span class="sxs-lookup"><span data-stu-id="cce2f-365">To achieve that, you have used Dependency Injection inside controllers, views and action filters.</span></span>

<span data-ttu-id="cce2f-366">Omówione zostały następujące pojęcia:</span><span class="sxs-lookup"><span data-stu-id="cce2f-366">The following concepts were covered:</span></span>

- <span data-ttu-id="cce2f-367">Funkcje wstrzykiwania zależności ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="cce2f-367">ASP.NET MVC 4 Dependency Injection features</span></span>
- <span data-ttu-id="cce2f-368">Integracja aparatu Unity przy użyciu pakietu NuGet Unity. Mvc3</span><span class="sxs-lookup"><span data-stu-id="cce2f-368">Unity integration using Unity.Mvc3 NuGet Package</span></span>
- <span data-ttu-id="cce2f-369">Iniekcja zależności w kontrolerach</span><span class="sxs-lookup"><span data-stu-id="cce2f-369">Dependency Injection in Controllers</span></span>
- <span data-ttu-id="cce2f-370">Iniekcja zależności w widokach</span><span class="sxs-lookup"><span data-stu-id="cce2f-370">Dependency Injection in Views</span></span>
- <span data-ttu-id="cce2f-371">Iniekcja zależności filtrów akcji</span><span class="sxs-lookup"><span data-stu-id="cce2f-371">Dependency injection of Action Filters</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="cce2f-372">Dodatek A: Instalowanie Visual Studio Express 2012 dla sieci Web</span><span class="sxs-lookup"><span data-stu-id="cce2f-372">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="cce2f-373">Można zainstalować **Microsoft Visual Studio Express 2012 dla sieci Web** lub innej &quot;Express&quot; wersji za pomocą **[Instalator platformy Microsoft Web](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="cce2f-373">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="cce2f-374">Poniższe instrukcje przeprowadzą Cię przez kroki wymagane do zainstalowania *programu Visual Studio Express 2012 for Web* przy użyciu *Instalator platformy Microsoft Web*.</span><span class="sxs-lookup"><span data-stu-id="cce2f-374">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="cce2f-375">Przejdź do obszaru [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169) (Ustawienia — Integracje i usługi).</span><span class="sxs-lookup"><span data-stu-id="cce2f-375">Go to [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="cce2f-376">Alternatywnie, jeśli zainstalowano już Instalatora platformy sieci Web, można go otworzyć i wyszukać produkt &quot;<em>Visual Studio Express 2012 for Web przy użyciu zestawu Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="cce2f-376">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="cce2f-377">Kliknij pozycję **Zainstaluj teraz**.</span><span class="sxs-lookup"><span data-stu-id="cce2f-377">Click on **Install Now**.</span></span> <span data-ttu-id="cce2f-378">Jeśli nie masz **Instalatora platformy sieci Web** , nastąpi przekierowanie do pobrania i zainstalowania go w pierwszej kolejności.</span><span class="sxs-lookup"><span data-stu-id="cce2f-378">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="cce2f-379">Po otwarciu **Instalatora platformy sieci Web** kliknij przycisk **Zainstaluj** , aby rozpocząć instalację.</span><span class="sxs-lookup"><span data-stu-id="cce2f-379">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="cce2f-380">![Zainstaluj Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "Zainstaluj Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="cce2f-380">![Install Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="cce2f-381">*Zainstaluj Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="cce2f-381">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="cce2f-382">Przeczytaj wszystkie licencje i postanowienia produktów, a następnie kliknij przycisk **Akceptuję** , aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="cce2f-382">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akceptowanie postanowień licencyjnych](aspnet-mvc-4-dependency-injection/_static/image15.png)

    <span data-ttu-id="cce2f-384">*Akceptowanie postanowień licencyjnych*</span><span class="sxs-lookup"><span data-stu-id="cce2f-384">*Accepting the license terms*</span></span>
5. <span data-ttu-id="cce2f-385">Poczekaj na zakończenie procesu pobierania i instalacji.</span><span class="sxs-lookup"><span data-stu-id="cce2f-385">Wait until the downloading and installation process completes.</span></span>

    ![Postęp instalacji](aspnet-mvc-4-dependency-injection/_static/image16.png)

    <span data-ttu-id="cce2f-387">*Postęp instalacji*</span><span class="sxs-lookup"><span data-stu-id="cce2f-387">*Installation progress*</span></span>
6. <span data-ttu-id="cce2f-388">Po zakończeniu instalacji kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="cce2f-388">When the installation completes, click **Finish**.</span></span>

    ![Instalacja zakończona](aspnet-mvc-4-dependency-injection/_static/image17.png)

    <span data-ttu-id="cce2f-390">*Instalacja zakończona*</span><span class="sxs-lookup"><span data-stu-id="cce2f-390">*Installation completed*</span></span>
7. <span data-ttu-id="cce2f-391">Kliknij przycisk **Zakończ** , aby zamknąć Instalatora platformy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="cce2f-391">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="cce2f-392">Aby otworzyć Visual Studio Express dla sieci Web, przejdź do ekranu **startowego** i zacznij pisać &quot;**vs Express**&quot;, a następnie kliknij kafelek **vs Express for Web** .</span><span class="sxs-lookup"><span data-stu-id="cce2f-392">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Kafelek VS Express for Web](aspnet-mvc-4-dependency-injection/_static/image18.png)

    <span data-ttu-id="cce2f-394">*Kafelek VS Express for Web*</span><span class="sxs-lookup"><span data-stu-id="cce2f-394">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="cce2f-395">Dodatek B: używanie fragmentów kodu</span><span class="sxs-lookup"><span data-stu-id="cce2f-395">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="cce2f-396">Za pomocą fragmentów kodu masz cały kod, którego potrzebujesz na wyręką.</span><span class="sxs-lookup"><span data-stu-id="cce2f-396">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="cce2f-397">Dokument laboratoryjny powiedzie się dokładnie wtedy, gdy można z nich korzystać, jak pokazano na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="cce2f-397">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="cce2f-398">![Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio](aspnet-mvc-4-dependency-injection/_static/image19.png "Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio")</span><span class="sxs-lookup"><span data-stu-id="cce2f-398">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-dependency-injection/_static/image19.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="cce2f-399">*Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="cce2f-399">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="cce2f-400">***Aby dodać fragment kodu przy użyciu klawiatury (C# tylko)***</span><span class="sxs-lookup"><span data-stu-id="cce2f-400">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="cce2f-401">Umieść kursor, w którym chcesz wstawić kod.</span><span class="sxs-lookup"><span data-stu-id="cce2f-401">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="cce2f-402">Zacznij wpisywać nazwę fragmentu kodu (bez spacji lub łączników).</span><span class="sxs-lookup"><span data-stu-id="cce2f-402">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="cce2f-403">Obejrzyj jako IntelliSense wyświetla pasujące nazwy fragmentów kodu.</span><span class="sxs-lookup"><span data-stu-id="cce2f-403">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="cce2f-404">Wybierz poprawny fragment kodu (lub Kontynuuj wpisywanie, dopóki nie zostanie zaznaczona cała nazwa fragmentu kodu).</span><span class="sxs-lookup"><span data-stu-id="cce2f-404">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="cce2f-405">Naciśnij dwukrotnie klawisz Tab, aby wstawić fragment kodu w lokalizacji kursora.</span><span class="sxs-lookup"><span data-stu-id="cce2f-405">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="cce2f-406">![Zacznij wpisywać nazwę fragmentu kodu](aspnet-mvc-4-dependency-injection/_static/image20.png "Zacznij wpisywać nazwę fragmentu kodu")</span><span class="sxs-lookup"><span data-stu-id="cce2f-406">![Start typing the snippet name](aspnet-mvc-4-dependency-injection/_static/image20.png "Start typing the snippet name")</span></span>

<span data-ttu-id="cce2f-407">*Zacznij wpisywać nazwę fragmentu kodu*</span><span class="sxs-lookup"><span data-stu-id="cce2f-407">*Start typing the snippet name*</span></span>

<span data-ttu-id="cce2f-408">![Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu](aspnet-mvc-4-dependency-injection/_static/image21.png "Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu")</span><span class="sxs-lookup"><span data-stu-id="cce2f-408">![Press Tab to select the highlighted snippet](aspnet-mvc-4-dependency-injection/_static/image21.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="cce2f-409">*Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="cce2f-409">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="cce2f-410">![Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta](aspnet-mvc-4-dependency-injection/_static/image22.png "Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta")</span><span class="sxs-lookup"><span data-stu-id="cce2f-410">![Press Tab again and the snippet will expand](aspnet-mvc-4-dependency-injection/_static/image22.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="cce2f-411">*Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta*</span><span class="sxs-lookup"><span data-stu-id="cce2f-411">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="cce2f-412">***Aby dodać fragment kodu przy użyciu myszy (C#, Visual Basic i XML)*** jedno.</span><span class="sxs-lookup"><span data-stu-id="cce2f-412">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="cce2f-413">Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu.</span><span class="sxs-lookup"><span data-stu-id="cce2f-413">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="cce2f-414">Wybierz **Wstaw fragment** kodu, po którym następuje **Moje fragmenty kodów**.</span><span class="sxs-lookup"><span data-stu-id="cce2f-414">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="cce2f-415">Wybierz odpowiedni fragment kodu z listy, klikając go.</span><span class="sxs-lookup"><span data-stu-id="cce2f-415">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="cce2f-416">![Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment](aspnet-mvc-4-dependency-injection/_static/image23.png "Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment")</span><span class="sxs-lookup"><span data-stu-id="cce2f-416">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-dependency-injection/_static/image23.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="cce2f-417">*Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment*</span><span class="sxs-lookup"><span data-stu-id="cce2f-417">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="cce2f-418">![Wybierz odpowiedni fragment kodu z listy, klikając go](aspnet-mvc-4-dependency-injection/_static/image24.png "Wybierz odpowiedni fragment kodu z listy, klikając go")</span><span class="sxs-lookup"><span data-stu-id="cce2f-418">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-dependency-injection/_static/image24.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="cce2f-419">*Wybierz odpowiedni fragment kodu z listy, klikając go*</span><span class="sxs-lookup"><span data-stu-id="cce2f-419">*Pick the relevant snippet from the list, by clicking on it*</span></span>
