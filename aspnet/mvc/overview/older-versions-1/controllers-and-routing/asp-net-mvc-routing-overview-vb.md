---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
title: Omówienie routingu ASP.NET MVC (VB) | Microsoft Docs
author: StephenWalther
description: W tym samouczku Stephen Walther pokazuje, jak platforma ASP.NET MVC mapuje żądania przeglądarki do akcji kontrolera.
ms.author: riande
ms.date: 08/19/2008
ms.assetid: 4bc8d19a-80f1-44b4-adbf-95ed22d691ca
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: ed043d76b89ce31945cf3423b0c5afca9383cc21
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601536"
---
# <a name="aspnet-mvc-routing-overview-vb"></a><span data-ttu-id="9163a-103">Omówienie routingu we wzorcu ASP.NET MVC (VB)</span><span class="sxs-lookup"><span data-stu-id="9163a-103">ASP.NET MVC Routing Overview (VB)</span></span>

<span data-ttu-id="9163a-104">Autor [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="9163a-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="9163a-105">W tym samouczku Stephen Walther pokazuje, jak platforma ASP.NET MVC mapuje żądania przeglądarki do akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="9163a-105">In this tutorial, Stephen Walther shows how the ASP.NET MVC framework maps browser requests to controller actions.</span></span>

<span data-ttu-id="9163a-106">W tym samouczku wprowadzasz do ważnej funkcji każdej aplikacji ASP.NET MVC o nazwie *ASP.NET routing*.</span><span class="sxs-lookup"><span data-stu-id="9163a-106">In this tutorial, you are introduced to an important feature of every ASP.NET MVC application called *ASP.NET Routing*.</span></span> <span data-ttu-id="9163a-107">Moduł routingu ASP.NET jest odpowiedzialny za mapowanie przychodzących żądań przeglądarki do określonych akcji kontrolera MVC.</span><span class="sxs-lookup"><span data-stu-id="9163a-107">The ASP.NET Routing module is responsible for mapping incoming browser requests to particular MVC controller actions.</span></span> <span data-ttu-id="9163a-108">Na końcu tego samouczka dowiesz się, jak standardowa tabela tras mapuje żądania na akcje kontrolera.</span><span class="sxs-lookup"><span data-stu-id="9163a-108">By the end of this tutorial, you will understand how the standard route table maps requests to controller actions.</span></span>

## <a name="using-the-default-route-table"></a><span data-ttu-id="9163a-109">Korzystanie z tabeli tras domyślnych</span><span class="sxs-lookup"><span data-stu-id="9163a-109">Using the Default Route Table</span></span>

<span data-ttu-id="9163a-110">Podczas tworzenia nowej aplikacji ASP.NET MVC aplikacja jest już skonfigurowana do używania routingu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9163a-110">When you create a new ASP.NET MVC application, the application is already configured to use ASP.NET Routing.</span></span> <span data-ttu-id="9163a-111">Routing ASP.NET jest skonfigurowany w dwóch miejscach.</span><span class="sxs-lookup"><span data-stu-id="9163a-111">ASP.NET Routing is setup in two places.</span></span>

<span data-ttu-id="9163a-112">Najpierw ASP.NET routing jest włączony w pliku konfiguracji sieci Web aplikacji (plik Web. config).</span><span class="sxs-lookup"><span data-stu-id="9163a-112">First, ASP.NET Routing is enabled in your application's Web configuration file (Web.config file).</span></span> <span data-ttu-id="9163a-113">W pliku konfiguracyjnym znajdują się cztery sekcje dotyczące routingu: sekcja system. Web. httpModules, sekcja system. Web. httpHandlers, sekcja system. WebServer. module oraz sekcja system. WebServer. programy obsługi.</span><span class="sxs-lookup"><span data-stu-id="9163a-113">There are four sections in the configuration file that are relevant to routing: the system.web.httpModules section, the system.web.httpHandlers section, the system.webserver.modules section, and the system.webserver.handlers section.</span></span> <span data-ttu-id="9163a-114">Należy zachować ostrożność, aby nie usuwać tych sekcji, ponieważ nie będą już działać.</span><span class="sxs-lookup"><span data-stu-id="9163a-114">Be careful not to delete these sections because without these sections routing will no longer work.</span></span>

<span data-ttu-id="9163a-115">Drugi i co ważniejsze, tabela tras jest tworzona w pliku Global. asax aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9163a-115">Second, and more importantly, a route table is created in the application's Global.asax file.</span></span> <span data-ttu-id="9163a-116">Plik Global. asax to specjalny plik, który zawiera programy obsługi zdarzeń dla zdarzeń cyklu życia aplikacji ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9163a-116">The Global.asax file is a special file that contains event handlers for ASP.NET application lifecycle events.</span></span> <span data-ttu-id="9163a-117">Tabela tras jest tworzona podczas zdarzenia uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9163a-117">The route table is created during the Application Start event.</span></span>

<span data-ttu-id="9163a-118">Plik z listą 1 zawiera domyślny plik Global. asax dla aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="9163a-118">The file in Listing 1 contains the default Global.asax file for an ASP.NET MVC application.</span></span>

<span data-ttu-id="9163a-119">**Lista 1-Global. asax. vb**</span><span class="sxs-lookup"><span data-stu-id="9163a-119">**Listing 1 - Global.asax.vb**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample1.vb)]

<span data-ttu-id="9163a-120">Po pierwszym uruchomieniu aplikacji MVC wywoływana jest metoda\_Start () aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9163a-120">When an MVC application first starts, the Application\_Start() method is called.</span></span> <span data-ttu-id="9163a-121">Ta metoda z kolei wywołuje metodę RegisterRoutes ().</span><span class="sxs-lookup"><span data-stu-id="9163a-121">This method, in turn, calls the RegisterRoutes() method.</span></span> <span data-ttu-id="9163a-122">Metoda RegisterRoutes () tworzy tabelę tras.</span><span class="sxs-lookup"><span data-stu-id="9163a-122">The RegisterRoutes() method creates the route table.</span></span>

<span data-ttu-id="9163a-123">Domyślna tabela tras zawiera pojedynczą trasę (nazwa domyślna).</span><span class="sxs-lookup"><span data-stu-id="9163a-123">The default route table contains a single route (named Default).</span></span> <span data-ttu-id="9163a-124">Trasa domyślna mapuje pierwszy segment adresu URL na nazwę kontrolera, drugi segment adresu URL do akcji kontrolera i trzeci segment do parametru o nazwie **ID**.</span><span class="sxs-lookup"><span data-stu-id="9163a-124">The Default route maps the first segment of a URL to a controller name, the second segment of a URL to a controller action, and the third segment to a parameter named **id**.</span></span>

<span data-ttu-id="9163a-125">Załóżmy, że na pasku adresu przeglądarki sieci Web wprowadzisz następujący adres URL:</span><span class="sxs-lookup"><span data-stu-id="9163a-125">Imagine that you enter the following URL into your web browser's address bar:</span></span>

<span data-ttu-id="9163a-126">/Home/Index/3</span><span class="sxs-lookup"><span data-stu-id="9163a-126">/Home/Index/3</span></span>

<span data-ttu-id="9163a-127">Trasa domyślna mapuje ten adres URL na następujące parametry:</span><span class="sxs-lookup"><span data-stu-id="9163a-127">The Default route maps this URL to the following parameters:</span></span>

- <span data-ttu-id="9163a-128">Kontroler = Strona główna</span><span class="sxs-lookup"><span data-stu-id="9163a-128">controller = Home</span></span>

- <span data-ttu-id="9163a-129">Akcja = indeks</span><span class="sxs-lookup"><span data-stu-id="9163a-129">action = Index</span></span>

- <span data-ttu-id="9163a-130">id = 3</span><span class="sxs-lookup"><span data-stu-id="9163a-130">id = 3</span></span>

<span data-ttu-id="9163a-131">Gdy żądanie adresu URL/Home/Index/3, zostanie wykonany następujący kod:</span><span class="sxs-lookup"><span data-stu-id="9163a-131">When you request the URL /Home/Index/3, the following code is executed:</span></span>

<span data-ttu-id="9163a-132">HomeController.Index(3)</span><span class="sxs-lookup"><span data-stu-id="9163a-132">HomeController.Index(3)</span></span>

<span data-ttu-id="9163a-133">Domyślna trasa obejmuje wartości domyślne dla wszystkich trzech parametrów.</span><span class="sxs-lookup"><span data-stu-id="9163a-133">The Default route includes defaults for all three parameters.</span></span> <span data-ttu-id="9163a-134">Jeśli nie podasz kontrolera, parametr Controller domyślnie przyjmuje wartość **Home**.</span><span class="sxs-lookup"><span data-stu-id="9163a-134">If you don't supply a controller, then the controller parameter defaults to the value **Home**.</span></span> <span data-ttu-id="9163a-135">Jeśli nie podasz akcji, parametr Action domyślnie przyjmuje wartość **index**.</span><span class="sxs-lookup"><span data-stu-id="9163a-135">If you don't supply an action, the action parameter defaults to the value **Index**.</span></span> <span data-ttu-id="9163a-136">Na koniec, jeśli nie podasz identyfikatora, parametr identyfikatora zostanie domyślnie pustym ciągiem.</span><span class="sxs-lookup"><span data-stu-id="9163a-136">Finally, if you don't supply an id, the id parameter defaults to an empty string.</span></span>

<span data-ttu-id="9163a-137">Przyjrzyjmy się kilku przykładom, jak trasa domyślna mapuje adresy URL na akcje kontrolera.</span><span class="sxs-lookup"><span data-stu-id="9163a-137">Let's look at a few examples of how the Default route maps URLs to controller actions.</span></span> <span data-ttu-id="9163a-138">Załóżmy, że na pasku adresu przeglądarki wprowadzisz następujący adres URL:</span><span class="sxs-lookup"><span data-stu-id="9163a-138">Imagine that you enter the following URL into your browser address bar:</span></span>

<span data-ttu-id="9163a-139">/Home</span><span class="sxs-lookup"><span data-stu-id="9163a-139">/Home</span></span>

<span data-ttu-id="9163a-140">Ze względu na domyślne wartości domyślnych parametrów trasy wprowadzenie tego adresu URL spowoduje wywołanie metody index () klasy HomeController na liście 2.</span><span class="sxs-lookup"><span data-stu-id="9163a-140">Because of the Default route parameter defaults, entering this URL will cause the Index() method of the HomeController class in Listing 2 to be called.</span></span>

<span data-ttu-id="9163a-141">**Lista 2 — HomeController. vb**</span><span class="sxs-lookup"><span data-stu-id="9163a-141">**Listing 2 - HomeController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample2.vb)]

<span data-ttu-id="9163a-142">Na liście 2 Klasa HomeController zawiera metodę o nazwie index (), która akceptuje pojedynczy parametr o nazwie ID. /Home URL powoduje wywołanie metody index () z wartością Nothing jako wartość parametru ID.</span><span class="sxs-lookup"><span data-stu-id="9163a-142">In Listing 2, the HomeController class includes a method named Index() that accepts a single parameter named Id. The URL /Home causes the Index() method to be called with the value Nothing as the value of the Id parameter.</span></span>

<span data-ttu-id="9163a-143">Ze względu na sposób, w jaki Struktura MVC wywołuje akcje kontrolera, adres URL/Home również jest zgodny z metodą index () klasy HomeController na liście 3.</span><span class="sxs-lookup"><span data-stu-id="9163a-143">Because of the way that the MVC framework invokes controller actions, the URL /Home also matches the Index() method of the HomeController class in Listing 3.</span></span>

<span data-ttu-id="9163a-144">**Lista 3-HomeController. vb (Akcja indeksu bez parametrów)**</span><span class="sxs-lookup"><span data-stu-id="9163a-144">**Listing 3 - HomeController.vb (Index action with no parameter)**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample3.vb)]

<span data-ttu-id="9163a-145">Metoda index () na liście 3 nie akceptuje żadnych parametrów.</span><span class="sxs-lookup"><span data-stu-id="9163a-145">The Index() method in Listing 3 does not accept any parameters.</span></span> <span data-ttu-id="9163a-146">/Home URL spowoduje wywołanie tej metody index ().</span><span class="sxs-lookup"><span data-stu-id="9163a-146">The URL /Home will cause this Index() method to be called.</span></span> <span data-ttu-id="9163a-147">/Home/Index/3 adresu URL wywołuje również tę metodę (identyfikator jest ignorowany).</span><span class="sxs-lookup"><span data-stu-id="9163a-147">The URL /Home/Index/3 also invokes this method (the Id is ignored).</span></span>

<span data-ttu-id="9163a-148">/Home adresu URL jest również zgodna z metodą index () klasy HomeController na liście 4.</span><span class="sxs-lookup"><span data-stu-id="9163a-148">The URL /Home also matches the Index() method of the HomeController class in Listing 4.</span></span>

<span data-ttu-id="9163a-149">**Lista 4-HomeController. vb (Akcja indeksu z parametrem dopuszczającym wartość null)**</span><span class="sxs-lookup"><span data-stu-id="9163a-149">**Listing 4 - HomeController.vb (Index action with nullable parameter)**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample4.vb)]

<span data-ttu-id="9163a-150">W przypadku listy 4 Metoda index () ma jeden parametr Integer.</span><span class="sxs-lookup"><span data-stu-id="9163a-150">In Listing 4, the Index() method has one Integer parameter.</span></span> <span data-ttu-id="9163a-151">Ponieważ parametr jest parametrem wartości null (może mieć wartość Nothing), indeks () można wywołać bez zgłaszania błędu.</span><span class="sxs-lookup"><span data-stu-id="9163a-151">Because the parameter is a nullable parameter (can have the value Nothing), the Index() can be called without raising an error.</span></span>

<span data-ttu-id="9163a-152">Na koniec wywoływanie metody index () w liście 5 z adresem URL/Home powoduje wyjątek, ponieważ parametr ID *nie jest* parametrem dopuszczającym wartość null.</span><span class="sxs-lookup"><span data-stu-id="9163a-152">Finally, invoking the Index() method in Listing 5 with the URL /Home causes an exception since the Id parameter *is not* a nullable parameter.</span></span> <span data-ttu-id="9163a-153">Jeśli spróbujesz wywołać metodę index (), pojawi się komunikat o błędzie wyświetlany na rysunku 1.</span><span class="sxs-lookup"><span data-stu-id="9163a-153">If you attempt to invoke the Index() method then you get the error displayed in Figure 1.</span></span>

<span data-ttu-id="9163a-154">**Lista 5-HomeController. vb (Akcja indeksu z parametrem identyfikatora)**</span><span class="sxs-lookup"><span data-stu-id="9163a-154">**Listing 5 - HomeController.vb (Index action with Id parameter)**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample5.vb)]

<span data-ttu-id="9163a-155">[![wywoływania akcji kontrolera, która oczekuje wartości parametru](asp-net-mvc-routing-overview-vb/_static/image1.jpg)](asp-net-mvc-routing-overview-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9163a-155">[![Invoking a controller action that expects a parameter value](asp-net-mvc-routing-overview-vb/_static/image1.jpg)](asp-net-mvc-routing-overview-vb/_static/image1.png)</span></span>

<span data-ttu-id="9163a-156">**Ilustracja 01**: Wywoływanie akcji kontrolera, która oczekuje wartości parametru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](asp-net-mvc-routing-overview-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="9163a-156">**Figure 01**: Invoking a controller action that expects a parameter value ([Click to view full-size image](asp-net-mvc-routing-overview-vb/_static/image2.png))</span></span>

<span data-ttu-id="9163a-157">Adres URL/Home/Index/3, z drugiej strony, działa prawidłowo z akcją kontrolera indeksu w temacie 5.</span><span class="sxs-lookup"><span data-stu-id="9163a-157">The URL /Home/Index/3, on the other hand, works just fine with the Index controller action in Listing 5.</span></span> <span data-ttu-id="9163a-158">Żądanie/Home/Index/3 powoduje wywołanie metody index () z parametrem ID o wartości 3.</span><span class="sxs-lookup"><span data-stu-id="9163a-158">The request /Home/Index/3 causes the Index() method to be called with an Id parameter that has the value 3.</span></span>

## <a name="summary"></a><span data-ttu-id="9163a-159">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="9163a-159">Summary</span></span>

<span data-ttu-id="9163a-160">Celem tego samouczka było dostarczenie krótkiego wprowadzenia do routingu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9163a-160">The goal of this tutorial was to provide you with a brief introduction to ASP.NET Routing.</span></span> <span data-ttu-id="9163a-161">Zbadamy domyślną tabelę tras uzyskaną z nową aplikacją ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="9163a-161">We examined the default route table that you get with a new ASP.NET MVC application.</span></span> <span data-ttu-id="9163a-162">Wiesz już, jak trasa domyślna mapuje adresy URL na akcje kontrolera.</span><span class="sxs-lookup"><span data-stu-id="9163a-162">You learned how the default route maps URLs to controller actions.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9163a-163">[Poprzednie](creating-an-action-cs.md)
> [dalej](understanding-action-filters-vb.md)</span><span class="sxs-lookup"><span data-stu-id="9163a-163">[Previous](creating-an-action-cs.md)
[Next](understanding-action-filters-vb.md)</span></span>
