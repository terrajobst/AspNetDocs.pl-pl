---
uid: mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
title: Omówienie kontrolera MVC ASP.NET (C#) | Microsoft Docs
author: StephenWalther
description: W tym samouczku Stephen Walther wprowadza do ASP.NET kontrolerów MVC. Dowiesz się, jak tworzyć nowe kontrolery i zwracać różne typy zasobów akcji...
ms.author: riande
ms.date: 02/16/2008
ms.assetid: b985c49a-3668-455c-a366-f85f6bc64b12
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: 1a287b37742400a17c2ed53cfd00bfb053b4f3d2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78544115"
---
# <a name="aspnet-mvc-controller-overview-c"></a><span data-ttu-id="241e6-104">Omówienie kontrolera ASP.NET MVC (C#)</span><span class="sxs-lookup"><span data-stu-id="241e6-104">ASP.NET MVC Controller Overview (C#)</span></span>

<span data-ttu-id="241e6-105">Autor [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="241e6-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="241e6-106">W tym samouczku Stephen Walther wprowadza do ASP.NET kontrolerów MVC.</span><span class="sxs-lookup"><span data-stu-id="241e6-106">In this tutorial, Stephen Walther introduces you to ASP.NET MVC controllers.</span></span> <span data-ttu-id="241e6-107">Dowiesz się, jak tworzyć nowe kontrolery i zwracać różne typy wyników akcji.</span><span class="sxs-lookup"><span data-stu-id="241e6-107">You learn how to create new controllers and return different types of action results.</span></span>

<span data-ttu-id="241e6-108">W tym samouczku przedstawiono temat ASP.NET kontrolery MVC, akcje kontrolera i wyniki akcji.</span><span class="sxs-lookup"><span data-stu-id="241e6-108">This tutorial explores the topic of ASP.NET MVC controllers, controller actions, and action results.</span></span> <span data-ttu-id="241e6-109">Po ukończeniu tego samouczka dowiesz się, w jaki sposób kontrolery są używane do kontrolowania sposobu, w jaki Goście współdziałają z witryną sieci Web ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="241e6-109">After you complete this tutorial, you will understand how controllers are used to control the way a visitor interacts with an ASP.NET MVC website.</span></span>

## <a name="understanding-controllers"></a><span data-ttu-id="241e6-110">Omówienie kontrolerów</span><span class="sxs-lookup"><span data-stu-id="241e6-110">Understanding Controllers</span></span>

<span data-ttu-id="241e6-111">Kontrolery MVC są odpowiedzialne za odpowiadanie na żądania wysyłane do witryny sieci Web ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="241e6-111">MVC controllers are responsible for responding to requests made against an ASP.NET MVC website.</span></span> <span data-ttu-id="241e6-112">Każde żądanie przeglądarki jest zamapowane na określony kontroler.</span><span class="sxs-lookup"><span data-stu-id="241e6-112">Each browser request is mapped to a particular controller.</span></span> <span data-ttu-id="241e6-113">Załóżmy na przykład, że wprowadzasz następujący adres URL na pasku adresu przeglądarki:</span><span class="sxs-lookup"><span data-stu-id="241e6-113">For example, imagine that you enter the following URL into the address bar of your browser:</span></span>

`http://localhost/Product/Index/3`

<span data-ttu-id="241e6-114">W takim przypadku jest wywoływany kontroler o nazwie ProductController.</span><span class="sxs-lookup"><span data-stu-id="241e6-114">In this case, a controller named ProductController is invoked.</span></span> <span data-ttu-id="241e6-115">ProductController jest odpowiedzialny za generowanie odpowiedzi na żądanie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="241e6-115">The ProductController is responsible for generating the response to the browser request.</span></span> <span data-ttu-id="241e6-116">Na przykład kontroler może zwrócić określony widok z powrotem do przeglądarki lub kontroler może przekierować użytkownika do innego kontrolera.</span><span class="sxs-lookup"><span data-stu-id="241e6-116">For example, the controller might return a particular view back to the browser or the controller might redirect the user to another controller.</span></span>

<span data-ttu-id="241e6-117">Lista 1 zawiera prosty kontroler o nazwie ProductController.</span><span class="sxs-lookup"><span data-stu-id="241e6-117">Listing 1 contains a simple controller named ProductController.</span></span>

<span data-ttu-id="241e6-118">**Listing1 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="241e6-118">**Listing1 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample1.cs)]

<span data-ttu-id="241e6-119">Jak widać na liście 1, kontroler jest tylko klasą (Visual Basic .NET lub C# klasą).</span><span class="sxs-lookup"><span data-stu-id="241e6-119">As you can see from Listing 1, a controller is just a class (a Visual Basic .NET or C# class).</span></span> <span data-ttu-id="241e6-120">Kontroler jest klasą pochodzącą od podstawowej klasy System. Web. MVC. Controller.</span><span class="sxs-lookup"><span data-stu-id="241e6-120">A controller is a class that derives from the base System.Web.Mvc.Controller class.</span></span> <span data-ttu-id="241e6-121">Ponieważ kontroler dziedziczy z tej klasy bazowej, kontroler dziedziczy kilka przydatnych metod bezpłatnie (omawiamy te metody w chwilę).</span><span class="sxs-lookup"><span data-stu-id="241e6-121">Because a controller inherits from this base class, a controller inherits several useful methods for free (We discuss these methods in a moment).</span></span>

## <a name="understanding-controller-actions"></a><span data-ttu-id="241e6-122">Informacje o akcjach kontrolera</span><span class="sxs-lookup"><span data-stu-id="241e6-122">Understanding Controller Actions</span></span>

<span data-ttu-id="241e6-123">Kontroler ujawnia akcje kontrolera.</span><span class="sxs-lookup"><span data-stu-id="241e6-123">A controller exposes controller actions.</span></span> <span data-ttu-id="241e6-124">Akcja to metoda na kontrolerze, który jest wywoływany po wprowadzeniu określonego adresu URL na pasku adresu przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="241e6-124">An action is a method on a controller that gets called when you enter a particular URL in your browser address bar.</span></span> <span data-ttu-id="241e6-125">Załóżmy na przykład, że tworzysz żądanie dla następującego adresu URL:</span><span class="sxs-lookup"><span data-stu-id="241e6-125">For example, imagine that you make a request for the following URL:</span></span>

`http://localhost/Product/Index/3`

<span data-ttu-id="241e6-126">W tym przypadku Metoda index () jest wywoływana w klasie ProductController.</span><span class="sxs-lookup"><span data-stu-id="241e6-126">In this case, the Index() method is called on the ProductController class.</span></span> <span data-ttu-id="241e6-127">Metoda index () jest przykładem akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="241e6-127">The Index() method is an example of a controller action.</span></span>

<span data-ttu-id="241e6-128">Akcja kontrolera musi być publiczną metodą klasy kontrolera.</span><span class="sxs-lookup"><span data-stu-id="241e6-128">A controller action must be a public method of a controller class.</span></span> <span data-ttu-id="241e6-129">C#Metody domyślnie są metodami prywatnymi.</span><span class="sxs-lookup"><span data-stu-id="241e6-129">C# methods, by default, are private methods.</span></span> <span data-ttu-id="241e6-130">Należy pamiętać, że każda metoda publiczna dodawana do klasy kontrolera jest automatycznie uwidaczniana jako akcja kontrolera (należy zachować ostrożność, ponieważ działanie kontrolera może być wywoływane przez każdą z nich, wpisując prawidłowy adres URL na pasku adresu przeglądarki).</span><span class="sxs-lookup"><span data-stu-id="241e6-130">Realize that any public method that you add to a controller class is exposed as a controller action automatically (You must be careful about this since a controller action can be invoked by anyone in the universe simply by typing the right URL into a browser address bar).</span></span>

<span data-ttu-id="241e6-131">Istnieją pewne dodatkowe wymagania, które muszą być spełnione przez akcję kontrolera.</span><span class="sxs-lookup"><span data-stu-id="241e6-131">There are some additional requirements that must be satisfied by a controller action.</span></span> <span data-ttu-id="241e6-132">Metoda używana jako akcja kontrolera nie może być przeciążona.</span><span class="sxs-lookup"><span data-stu-id="241e6-132">A method used as a controller action cannot be overloaded.</span></span> <span data-ttu-id="241e6-133">Ponadto akcja kontrolera nie może być metodą statyczną.</span><span class="sxs-lookup"><span data-stu-id="241e6-133">Furthermore, a controller action cannot be a static method.</span></span> <span data-ttu-id="241e6-134">Oprócz tego, można użyć tylko dowolnej metody jako akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="241e6-134">Other than that, you can use just about any method as a controller action.</span></span>

## <a name="understanding-action-results"></a><span data-ttu-id="241e6-135">Informacje o wynikach akcji</span><span class="sxs-lookup"><span data-stu-id="241e6-135">Understanding Action Results</span></span>

<span data-ttu-id="241e6-136">Akcja kontrolera zwraca coś o nazwie " *wynik akcji*".</span><span class="sxs-lookup"><span data-stu-id="241e6-136">A controller action returns something called an *action result*.</span></span> <span data-ttu-id="241e6-137">Wynikiem akcji jest to, co akcja kontrolera zwraca w odpowiedzi na żądanie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="241e6-137">An action result is what a controller action returns in response to a browser request.</span></span>

<span data-ttu-id="241e6-138">Platforma ASP.NET MVC obsługuje kilka typów wyników akcji, takich jak:</span><span class="sxs-lookup"><span data-stu-id="241e6-138">The ASP.NET MVC framework supports several types of action results including:</span></span>

1. <span data-ttu-id="241e6-139">ViewResult — reprezentuje kod HTML i znacznik.</span><span class="sxs-lookup"><span data-stu-id="241e6-139">ViewResult - Represents HTML and markup.</span></span>
2. <span data-ttu-id="241e6-140">EmptyResult — reprezentuje brak wyniku.</span><span class="sxs-lookup"><span data-stu-id="241e6-140">EmptyResult - Represents no result.</span></span>
3. <span data-ttu-id="241e6-141">RedirectResult — reprezentuje przekierowanie do nowego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="241e6-141">RedirectResult - Represents a redirection to a new URL.</span></span>
4. <span data-ttu-id="241e6-142">JsonResult — reprezentuje wynik JavaScript Object Notation, który może być używany w aplikacji AJAX.</span><span class="sxs-lookup"><span data-stu-id="241e6-142">JsonResult - Represents a JavaScript Object Notation result that can be used in an AJAX application.</span></span>
5. <span data-ttu-id="241e6-143">JavaScriptResult — reprezentuje skrypt JavaScript.</span><span class="sxs-lookup"><span data-stu-id="241e6-143">JavaScriptResult - Represents a JavaScript script.</span></span>
6. <span data-ttu-id="241e6-144">ContentResult — reprezentuje wynik tekstowy.</span><span class="sxs-lookup"><span data-stu-id="241e6-144">ContentResult - Represents a text result.</span></span>
7. <span data-ttu-id="241e6-145">FileContentResult — reprezentuje plik do pobrania (z zawartością binarną).</span><span class="sxs-lookup"><span data-stu-id="241e6-145">FileContentResult - Represents a downloadable file (with the binary content).</span></span>
8. <span data-ttu-id="241e6-146">FilePathResult — reprezentuje plik do pobrania (ze ścieżką).</span><span class="sxs-lookup"><span data-stu-id="241e6-146">FilePathResult - Represents a downloadable file (with a path).</span></span>
9. <span data-ttu-id="241e6-147">FileStreamResult — reprezentuje plik do pobrania (ze strumieniem pliku).</span><span class="sxs-lookup"><span data-stu-id="241e6-147">FileStreamResult - Represents a downloadable file (with a file stream).</span></span>

<span data-ttu-id="241e6-148">Wszystkie te wyniki akcji dziedziczą z podstawowej klasy ActionResult.</span><span class="sxs-lookup"><span data-stu-id="241e6-148">All of these action results inherit from the base ActionResult class.</span></span>

<span data-ttu-id="241e6-149">W większości przypadków akcja kontrolera zwraca ViewResult.</span><span class="sxs-lookup"><span data-stu-id="241e6-149">In most cases, a controller action returns a ViewResult.</span></span> <span data-ttu-id="241e6-150">Na przykład akcja kontrolera indeksu w liście 2 zwraca ViewResult.</span><span class="sxs-lookup"><span data-stu-id="241e6-150">For example, the Index controller action in Listing 2 returns a ViewResult.</span></span>

<span data-ttu-id="241e6-151">**Lista 2 — Controllers\BookController.cs**</span><span class="sxs-lookup"><span data-stu-id="241e6-151">**Listing 2 - Controllers\BookController.cs**</span></span>

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample2.cs)]

<span data-ttu-id="241e6-152">Gdy akcja zwraca ViewResult, kod HTML jest zwracany do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="241e6-152">When an action returns a ViewResult, HTML is returned to the browser.</span></span> <span data-ttu-id="241e6-153">Metoda index () w liście 2 zwraca widok o nazwie index do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="241e6-153">The Index() method in Listing 2 returns a view named Index to the browser.</span></span>

<span data-ttu-id="241e6-154">Zwróć uwagę, że akcja index () na liście 2 nie zwraca elementu ViewResult ().</span><span class="sxs-lookup"><span data-stu-id="241e6-154">Notice that the Index() action in Listing 2 does not return a ViewResult().</span></span> <span data-ttu-id="241e6-155">Zamiast tego Metoda View () klasy bazowej kontrolera jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="241e6-155">Instead, the View() method of the Controller base class is called.</span></span> <span data-ttu-id="241e6-156">Zwykle nie zwracasz wyniku działania bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="241e6-156">Normally, you do not return an action result directly.</span></span> <span data-ttu-id="241e6-157">Zamiast tego należy wywołać jedną z następujących metod klasy podstawowej kontrolera:</span><span class="sxs-lookup"><span data-stu-id="241e6-157">Instead, you call one of the following methods of the Controller base class:</span></span>

1. <span data-ttu-id="241e6-158">Widok — zwraca wynik akcji ViewResult.</span><span class="sxs-lookup"><span data-stu-id="241e6-158">View - Returns a ViewResult action result.</span></span>
2. <span data-ttu-id="241e6-159">Redirect — zwraca wynik akcji RedirectResult.</span><span class="sxs-lookup"><span data-stu-id="241e6-159">Redirect - Returns a RedirectResult action result.</span></span>
3. <span data-ttu-id="241e6-160">RedirectToAction — zwraca wynik akcji RedirectToRouteResult.</span><span class="sxs-lookup"><span data-stu-id="241e6-160">RedirectToAction - Returns a RedirectToRouteResult action result.</span></span>
4. <span data-ttu-id="241e6-161">RedirectToRoute — zwraca wynik akcji RedirectToRouteResult.</span><span class="sxs-lookup"><span data-stu-id="241e6-161">RedirectToRoute - Returns a RedirectToRouteResult action result.</span></span>
5. <span data-ttu-id="241e6-162">JSON — zwraca wynik akcji JsonResult.</span><span class="sxs-lookup"><span data-stu-id="241e6-162">Json - Returns a JsonResult action result.</span></span>
6. <span data-ttu-id="241e6-163">JavaScriptResult — zwraca JavaScriptResult.</span><span class="sxs-lookup"><span data-stu-id="241e6-163">JavaScriptResult - Returns a JavaScriptResult.</span></span>
7. <span data-ttu-id="241e6-164">Content — zwraca wynik akcji ContentResult.</span><span class="sxs-lookup"><span data-stu-id="241e6-164">Content - Returns a ContentResult action result.</span></span>
8. <span data-ttu-id="241e6-165">Plik — zwraca FileContentResult, FilePathResult lub FileStreamResult w zależności od parametrów przekazaną do metody.</span><span class="sxs-lookup"><span data-stu-id="241e6-165">File - Returns a FileContentResult, FilePathResult, or FileStreamResult depending on the parameters passed to the method.</span></span>

<span data-ttu-id="241e6-166">Tak więc, jeśli chcesz zwrócić widok do przeglądarki, wywoływana jest metoda View ().</span><span class="sxs-lookup"><span data-stu-id="241e6-166">So, if you want to return a View to the browser, you call the View() method.</span></span> <span data-ttu-id="241e6-167">Jeśli chcesz przekierować użytkownika z jednej akcji kontrolera do innej, należy wywołać metodę RedirectToAction ().</span><span class="sxs-lookup"><span data-stu-id="241e6-167">If you want to redirect the user from one controller action to another, you call the RedirectToAction() method.</span></span> <span data-ttu-id="241e6-168">Na przykład akcja szczegóły () na liście 3 wyświetla widok lub przekierowuje użytkownika do akcji index () w zależności od tego, czy parametr ID ma wartość.</span><span class="sxs-lookup"><span data-stu-id="241e6-168">For example, the Details() action in Listing 3 either displays a view or redirects the user to the Index() action depending on whether the Id parameter has a value.</span></span>

<span data-ttu-id="241e6-169">**Lista 3 — CustomerController.cs**</span><span class="sxs-lookup"><span data-stu-id="241e6-169">**Listing 3 - CustomerController.cs**</span></span>

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample3.cs)]

<span data-ttu-id="241e6-170">Wynik akcji ContentResult jest specjalny.</span><span class="sxs-lookup"><span data-stu-id="241e6-170">The ContentResult action result is special.</span></span> <span data-ttu-id="241e6-171">Możesz użyć wyniku działania ContentResult, aby zwrócić wynik akcji w postaci zwykłego tekstu.</span><span class="sxs-lookup"><span data-stu-id="241e6-171">You can use the ContentResult action result to return an action result as plain text.</span></span> <span data-ttu-id="241e6-172">Na przykład Metoda index () w liście 4 zwraca komunikat w postaci zwykłego tekstu, a nie jako HTML.</span><span class="sxs-lookup"><span data-stu-id="241e6-172">For example, the Index() method in Listing 4 returns a message as plain text and not as HTML.</span></span>

<span data-ttu-id="241e6-173">**Lista 4 — Controllers\StatusController.cs**</span><span class="sxs-lookup"><span data-stu-id="241e6-173">**Listing 4 - Controllers\StatusController.cs**</span></span>

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample4.cs)]

<span data-ttu-id="241e6-174">Po wywołaniu akcji StatusController. index () widok nie jest zwracany.</span><span class="sxs-lookup"><span data-stu-id="241e6-174">When the StatusController.Index() action is invoked, a view is not returned.</span></span> <span data-ttu-id="241e6-175">Zamiast tego tekst nieprzetworzony "Hello world!"</span><span class="sxs-lookup"><span data-stu-id="241e6-175">Instead, the raw text "Hello World!"</span></span> <span data-ttu-id="241e6-176">jest zwracany do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="241e6-176">is returned to the browser.</span></span>

<span data-ttu-id="241e6-177">Jeśli akcja kontrolera zwraca wynik, który nie jest wynikiem akcji — na przykład datę lub liczbę całkowitą, wynik jest zawijany automatycznie w ContentResult.</span><span class="sxs-lookup"><span data-stu-id="241e6-177">If a controller action returns a result that is not an action result - for example, a date or an integer - then the result is wrapped in a ContentResult automatically.</span></span> <span data-ttu-id="241e6-178">Na przykład po wywołaniu akcji index () elementu WorkController na liście 5 jest to data, która jest automatycznie zwracana jako ContentResult.</span><span class="sxs-lookup"><span data-stu-id="241e6-178">For example, when the Index() action of the WorkController in Listing 5 is invoked, the date is returned as a ContentResult automatically.</span></span>

<span data-ttu-id="241e6-179">**Lista 5 — WorkController.cs**</span><span class="sxs-lookup"><span data-stu-id="241e6-179">**Listing 5 - WorkController.cs**</span></span>

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample5.cs)]

<span data-ttu-id="241e6-180">Akcja index () w liście 5 zwraca obiekt DateTime.</span><span class="sxs-lookup"><span data-stu-id="241e6-180">The Index() action in Listing 5 returns a DateTime object.</span></span> <span data-ttu-id="241e6-181">Struktura ASP.NET MVC Konwertuje obiekt DateTime na ciąg i automatycznie zawija wartość DateTime w ContentResult.</span><span class="sxs-lookup"><span data-stu-id="241e6-181">The ASP.NET MVC framework converts the DateTime object to a string and wraps the DateTime value in a ContentResult automatically.</span></span> <span data-ttu-id="241e6-182">Przeglądarka otrzymuje datę i godzinę w postaci zwykłego tekstu.</span><span class="sxs-lookup"><span data-stu-id="241e6-182">The browser receives the date and time as plain text.</span></span>

## <a name="summary"></a><span data-ttu-id="241e6-183">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="241e6-183">Summary</span></span>

<span data-ttu-id="241e6-184">Celem tego samouczka było wprowadzenie do koncepcji kontrolerów MVC ASP.NET, akcji kontrolera i wyników akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="241e6-184">The purpose of this tutorial was to introduce you to the concepts of ASP.NET MVC controllers, controller actions, and controller action results.</span></span> <span data-ttu-id="241e6-185">W pierwszej sekcji przedstawiono sposób dodawania nowych kontrolerów do projektu ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="241e6-185">In the first section, you learned how to add new controllers to an ASP.NET MVC project.</span></span> <span data-ttu-id="241e6-186">Następnie wiesz, jak publiczne metody kontrolera są ujawniane w programie Universe jako akcje kontrolera.</span><span class="sxs-lookup"><span data-stu-id="241e6-186">Next, you learned how public methods of a controller are exposed to the universe as controller actions.</span></span> <span data-ttu-id="241e6-187">Na koniec omawiamy różne typy wyników akcji, które mogą być zwracane z akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="241e6-187">Finally, we discussed the different types of action results that can be returned from a controller action.</span></span> <span data-ttu-id="241e6-188">W szczególności omówione zostało zwrócenie elementu ViewResult, RedirectToActionResult i ContentResult z akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="241e6-188">In particular, we discussed how to return a ViewResult, RedirectToActionResult, and ContentResult from a controller action.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="241e6-189">[Poprzednie](creating-an-action-vb.md)
> [dalej](creating-custom-routes-cs.md)</span><span class="sxs-lookup"><span data-stu-id="241e6-189">[Previous](creating-an-action-vb.md)
[Next](creating-custom-routes-cs.md)</span></span>
