---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: Informacje o procesie wykonywania ASP.NET MVC | Microsoft Docs
author: microsoft
description: Dowiedz się, w jaki sposób platforma MVC ASP.NET przetwarza żądanie przeglądarki, krok po kroku.
ms.author: riande
ms.date: 01/27/2009
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: 28940947253e0af43886cf1231f8aaf4615526cc
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78541518"
---
# <a name="understanding-the-aspnet-mvc-execution-process"></a><span data-ttu-id="5b8b5-103">Objaśnienie procesu wykonywania we wzorcu ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="5b8b5-103">Understanding the ASP.NET MVC Execution Process</span></span>

<span data-ttu-id="5b8b5-104">przez [firmę Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="5b8b5-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="5b8b5-105">Dowiedz się, w jaki sposób platforma MVC ASP.NET przetwarza żądanie przeglądarki, krok po kroku.</span><span class="sxs-lookup"><span data-stu-id="5b8b5-105">Learn how the ASP.NET MVC framework processes a browser request step-by-step.</span></span>

<span data-ttu-id="5b8b5-106">Żądania do aplikacji sieci Web opartej na ASP.NET MVC najpierw przekazywane przez obiekt **UrlRoutingModule** , który jest modułem http.</span><span class="sxs-lookup"><span data-stu-id="5b8b5-106">Requests to an ASP.NET MVC-based Web application first pass through the **UrlRoutingModule** object, which is an HTTP module.</span></span> <span data-ttu-id="5b8b5-107">Ten moduł analizuje żądanie i wykonuje wybór trasy.</span><span class="sxs-lookup"><span data-stu-id="5b8b5-107">This module parses the request and performs route selection.</span></span> <span data-ttu-id="5b8b5-108">Obiekt **UrlRoutingModule** wybiera pierwszy obiekt trasy, który jest zgodny z bieżącym żądaniem.</span><span class="sxs-lookup"><span data-stu-id="5b8b5-108">The **UrlRoutingModule** object selects the first route object that matches the current request.</span></span> <span data-ttu-id="5b8b5-109">(Obiekt trasy jest klasą, która implementuje **RouteBase**, i jest zwykle wystąpieniem klasy **trasy** ). Jeśli żadne trasy nie pasują do siebie, obiekt **UrlRoutingModule** nie robi nic i pozwala na żądanie powraca do regularnego przetwarzania żądań ASP.NET lub IIS.</span><span class="sxs-lookup"><span data-stu-id="5b8b5-109">(A route object is a class that implements **RouteBase**, and is typically an instance of the **Route** class.) If no routes match, the **UrlRoutingModule** object does nothing and lets the request fall back to the regular ASP.NET or IIS request processing.</span></span>

<span data-ttu-id="5b8b5-110">Z wybranego obiektu **trasy** obiekt **UrlRoutingModule** uzyskuje obiekt **IRouteHandler** , który jest skojarzony z obiektem **Route** .</span><span class="sxs-lookup"><span data-stu-id="5b8b5-110">From the selected **Route** object, the **UrlRoutingModule** object obtains the **IRouteHandler** object that is associated with the **Route** object.</span></span> <span data-ttu-id="5b8b5-111">Zazwyczaj w aplikacji MVC będzie to wystąpienie **MvcRouteHandler**.</span><span class="sxs-lookup"><span data-stu-id="5b8b5-111">Typically, in an MVC application, this will be an instance of **MvcRouteHandler**.</span></span> <span data-ttu-id="5b8b5-112">Wystąpienie **IRouteHandler** tworzy obiekt **IHttpHandler** i przekazuje go do obiektu **IHttpContext** .</span><span class="sxs-lookup"><span data-stu-id="5b8b5-112">The **IRouteHandler** instance creates an **IHttpHandler** object and passes it the **IHttpContext** object.</span></span> <span data-ttu-id="5b8b5-113">Domyślnie wystąpienie **IHttpHandler** dla MVC jest obiektem **MvcHandler** .</span><span class="sxs-lookup"><span data-stu-id="5b8b5-113">By default, the **IHttpHandler** instance for MVC is the **MvcHandler** object.</span></span> <span data-ttu-id="5b8b5-114">Obiekt **MvcHandler** wybiera kontroler, który ostatecznie obsłuży żądanie.</span><span class="sxs-lookup"><span data-stu-id="5b8b5-114">The **MvcHandler** object then selects the controller that will ultimately handle the request.</span></span>

> [!NOTE]
> <span data-ttu-id="5b8b5-115">Gdy aplikacja sieci Web ASP.NET MVC jest uruchamiana w usługach IIS 7,0, dla projektów MVC nie jest wymagane rozszerzenie nazwy pliku.</span><span class="sxs-lookup"><span data-stu-id="5b8b5-115">When an ASP.NET MVC Web application runs in IIS 7.0, no file name extension is required for MVC projects.</span></span> <span data-ttu-id="5b8b5-116">Jednak w usługach IIS 6,0 program obsługi wymaga zamapowania rozszerzenia nazwy pliku MVC na ASP.NET ISAPI DLL.</span><span class="sxs-lookup"><span data-stu-id="5b8b5-116">However, in IIS 6.0, the handler requires that you map the .mvc file name extension to the ASP.NET ISAPI DLL.</span></span>

<span data-ttu-id="5b8b5-117">Moduł i program obsługi są punktami wejścia do struktury ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="5b8b5-117">The module and handler are the entry points to the ASP.NET MVC framework.</span></span> <span data-ttu-id="5b8b5-118">Wykonują następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="5b8b5-118">They perform the following actions:</span></span>

- <span data-ttu-id="5b8b5-119">Wybierz odpowiedni kontroler w aplikacji sieci Web MVC.</span><span class="sxs-lookup"><span data-stu-id="5b8b5-119">Select the appropriate controller in an MVC Web application.</span></span>
- <span data-ttu-id="5b8b5-120">Uzyskanie określonego wystąpienia kontrolera.</span><span class="sxs-lookup"><span data-stu-id="5b8b5-120">Obtain a specific controller instance.</span></span>
- <span data-ttu-id="5b8b5-121">Wywołaj metodę **Execute** kontrolera.</span><span class="sxs-lookup"><span data-stu-id="5b8b5-121">Call the controller's **Execute** method.</span></span>

<span data-ttu-id="5b8b5-122">Poniżej przedstawiono listę etapów wykonywania dla projektu sieci Web MVC:</span><span class="sxs-lookup"><span data-stu-id="5b8b5-122">The following lists the stages of execution for an MVC Web project:</span></span>

- <span data-ttu-id="5b8b5-123">Odbieraj pierwsze żądanie dla aplikacji</span><span class="sxs-lookup"><span data-stu-id="5b8b5-123">Receive first request for the application</span></span> 

    - <span data-ttu-id="5b8b5-124">W pliku Global. asax obiekty **tras** są dodawane do obiektu **Route** .</span><span class="sxs-lookup"><span data-stu-id="5b8b5-124">In the Global.asax file, **Route** objects are added to the **RouteTable** object.</span></span>
- <span data-ttu-id="5b8b5-125">Wykonaj Routing</span><span class="sxs-lookup"><span data-stu-id="5b8b5-125">Perform routing</span></span> 

    - <span data-ttu-id="5b8b5-126">Moduł **UrlRoutingModule** używa pierwszego dopasowanego obiektu **trasy** w kolekcji **Routes** w celu utworzenia obiektu **RouteData** , który następnie używa do tworzenia obiektu **RequestContext** (**IHttpContext**).</span><span class="sxs-lookup"><span data-stu-id="5b8b5-126">The **UrlRoutingModule** module uses the first matching **Route** object in the **RouteTable** collection to create the **RouteData** object, which it then uses to create a **RequestContext** (**IHttpContext**) object.</span></span>
- <span data-ttu-id="5b8b5-127">Utwórz procedurę obsługi żądania MVC</span><span class="sxs-lookup"><span data-stu-id="5b8b5-127">Create MVC request handler</span></span> 

    - <span data-ttu-id="5b8b5-128">Obiekt **MvcRouteHandler** tworzy wystąpienie klasy **MvcHandler** i przekazuje je do wystąpienia **RequestContext** .</span><span class="sxs-lookup"><span data-stu-id="5b8b5-128">The **MvcRouteHandler** object creates an instance of the **MvcHandler** class and passes it the **RequestContext** instance.</span></span>
- <span data-ttu-id="5b8b5-129">Utwórz kontroler</span><span class="sxs-lookup"><span data-stu-id="5b8b5-129">Create controller</span></span> 

    - <span data-ttu-id="5b8b5-130">Obiekt **MvcHandler** używa wystąpienia **RequestContext** do identyfikowania obiektu **IControllerFactory** (zwykle wystąpienie klasy **DefaultControllerFactory** ) do utworzenia wystąpienia kontrolera za pomocą.</span><span class="sxs-lookup"><span data-stu-id="5b8b5-130">The **MvcHandler** object uses the **RequestContext** instance to identify the **IControllerFactory** object (typically an instance of the **DefaultControllerFactory** class) to create the controller instance with.</span></span>
- <span data-ttu-id="5b8b5-131">Wykonaj kontroler — wystąpienie **MvcHandler** wywołuje metodę **Execute** sterownika.</span><span class="sxs-lookup"><span data-stu-id="5b8b5-131">Execute controller - The **MvcHandler** instance calls the controller s **Execute** method.</span></span> |
- <span data-ttu-id="5b8b5-132">Akcja wywołania</span><span class="sxs-lookup"><span data-stu-id="5b8b5-132">Invoke action</span></span> 

    - <span data-ttu-id="5b8b5-133">Większość kontrolerów dziedziczy z klasy podstawowej **kontrolera** .</span><span class="sxs-lookup"><span data-stu-id="5b8b5-133">Most controllers inherit from the **Controller** base class.</span></span> <span data-ttu-id="5b8b5-134">Dla kontrolerów, które to powodują, obiekt **ControllerActionInvoker** , który jest skojarzony z kontrolerem określa metodę akcji klasy kontrolera do wywołania, a następnie wywołuje tę metodę.</span><span class="sxs-lookup"><span data-stu-id="5b8b5-134">For controllers that do so, the **ControllerActionInvoker** object that is associated with the controller determines which action method of the controller class to call, and then calls that method.</span></span>
- <span data-ttu-id="5b8b5-135">Wykonaj wynik</span><span class="sxs-lookup"><span data-stu-id="5b8b5-135">Execute result</span></span> 

    - <span data-ttu-id="5b8b5-136">Typową metodą akcji może być otrzymanie danych wejściowych użytkownika, przygotowanie odpowiednich danych odpowiedzi, a następnie wykonanie wyniku zwracając typ wyniku.</span><span class="sxs-lookup"><span data-stu-id="5b8b5-136">A typical action method might receive user input, prepare the appropriate response data, and then execute the result by returning a result type.</span></span> <span data-ttu-id="5b8b5-137">Wbudowane typy wyników, które mogą być wykonywane, obejmują: **ViewResult** (który renderuje widok i jest najczęściej używanym typem wyniku), **RedirectToRouteResult**, **RedirectResult**, **ContentResult**, **JsonResult**i **EmptyResult**.</span><span class="sxs-lookup"><span data-stu-id="5b8b5-137">The built-in result types that can be executed include the following: **ViewResult** (which renders a view and is the most-often used result type), **RedirectToRouteResult**, **RedirectResult**, **ContentResult**, **JsonResult**, and **EmptyResult**.</span></span>
