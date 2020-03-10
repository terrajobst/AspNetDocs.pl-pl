---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: Kontrolery testowania jednostek w programie ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: W tym temacie opisano niektóre konkretne techniki kontrolerów testowania jednostek w interfejsie Web API 2. Przed przeczytaniem tego tematu warto zapoznać się z jednostką samouczka...
ms.author: riande
ms.date: 06/11/2014
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: cdb1700537021e276669de1a9e0330a62659746c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554993"
---
# <a name="unit-testing-controllers-in-aspnet-web-api-2"></a><span data-ttu-id="18147-104">Kontrolery testów jednostkowych we wzorcu ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="18147-104">Unit Testing Controllers in ASP.NET Web API 2</span></span>

<span data-ttu-id="18147-105">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="18147-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="18147-106">W tym temacie opisano niektóre konkretne techniki kontrolerów testowania jednostek w interfejsie Web API 2.</span><span class="sxs-lookup"><span data-stu-id="18147-106">This topic describes some specific techniques for unit testing controllers in Web API 2.</span></span> <span data-ttu-id="18147-107">Przed zapisaniem tego tematu warto przeczytać artykuł dotyczący [testowania jednostkowego ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), który pokazuje, jak dodać projekt jednostki testowej do rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="18147-107">Before reading this topic, you might want to read the tutorial [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), which shows how to add a unit-test project to your solution.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="18147-108">Wersje oprogramowania używane w samouczku</span><span class="sxs-lookup"><span data-stu-id="18147-108">Software versions used in the tutorial</span></span>
>
> - [<span data-ttu-id="18147-109">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="18147-109">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - <span data-ttu-id="18147-110">Internetowy interfejs API 2</span><span class="sxs-lookup"><span data-stu-id="18147-110">Web API 2</span></span>
> - <span data-ttu-id="18147-111">[MOQ](https://github.com/Moq) 4.5.30</span><span class="sxs-lookup"><span data-stu-id="18147-111">[Moq](https://github.com/Moq) 4.5.30</span></span>

> [!NOTE]
> <span data-ttu-id="18147-112">Używam MOQ, ale to samo pomysł odnosi się do każdej struktury imitacji.</span><span class="sxs-lookup"><span data-stu-id="18147-112">I used Moq, but the same idea applies to any mocking framework.</span></span> <span data-ttu-id="18147-113">MOQ 4.5.30 (i nowsze) obsługuje program Visual Studio 2017, Roslyn i .NET 4,5 i nowsze wersje.</span><span class="sxs-lookup"><span data-stu-id="18147-113">Moq 4.5.30 (and later) supports Visual Studio 2017, Roslyn and .NET 4.5 and later versions.</span></span>

<span data-ttu-id="18147-114">Typowym wzorcem w testach jednostkowych jest &quot;porządkowanie&quot;z założeniami:</span><span class="sxs-lookup"><span data-stu-id="18147-114">A common pattern in unit tests is &quot;arrange-act-assert&quot;:</span></span>

- <span data-ttu-id="18147-115">Porządkowanie: Skonfiguruj wszystkie wymagania wstępne dla testu do uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="18147-115">Arrange: Set up any prerequisites for the test to run.</span></span>
- <span data-ttu-id="18147-116">Działanie: wykonaj test.</span><span class="sxs-lookup"><span data-stu-id="18147-116">Act: Perform the test.</span></span>
- <span data-ttu-id="18147-117">Potwierdzenie: Sprawdź, czy test zakończył się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="18147-117">Assert: Verify that the test succeeded.</span></span>

<span data-ttu-id="18147-118">W kroku Rozmieść często będziesz używać obiektów imitacji lub szczątkowych.</span><span class="sxs-lookup"><span data-stu-id="18147-118">In the arrange step, you will often use mock or stub objects.</span></span> <span data-ttu-id="18147-119">Pozwala to zminimalizować liczbę zależności, dlatego test koncentruje się na testowaniu jednego elementu.</span><span class="sxs-lookup"><span data-stu-id="18147-119">That minimizes the number of dependencies, so the test is focused on testing one thing.</span></span>

<span data-ttu-id="18147-120">Oto kilka rzeczy, które należy wykonać test jednostkowy na kontrolerach interfejsu API sieci Web:</span><span class="sxs-lookup"><span data-stu-id="18147-120">Here are some things that you should unit test in your Web API controllers:</span></span>

- <span data-ttu-id="18147-121">Akcja zwraca poprawny typ odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="18147-121">The action returns the correct type of response.</span></span>
- <span data-ttu-id="18147-122">Nieprawidłowe parametry zwracają poprawną odpowiedź na błąd.</span><span class="sxs-lookup"><span data-stu-id="18147-122">Invalid parameters return the correct error response.</span></span>
- <span data-ttu-id="18147-123">Akcja wywołuje poprawną metodę dla repozytorium lub warstwy usług.</span><span class="sxs-lookup"><span data-stu-id="18147-123">The action calls the correct method on the repository or service layer.</span></span>
- <span data-ttu-id="18147-124">Jeśli odpowiedź zawiera model domeny, Sprawdź typ modelu.</span><span class="sxs-lookup"><span data-stu-id="18147-124">If the response includes a domain model, verify the model type.</span></span>

<span data-ttu-id="18147-125">Są to niektóre ogólne elementy, które należy przetestować, ale te szczegóły zależą od implementacji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="18147-125">These are some of the general things to test, but the specifics depend on your controller implementation.</span></span> <span data-ttu-id="18147-126">W szczególności czynimy dużą różnicą, czy akcje kontrolera zwracają **HttpResponseMessage** lub **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="18147-126">In particular, it makes a big difference whether your controller actions return **HttpResponseMessage** or **IHttpActionResult**.</span></span> <span data-ttu-id="18147-127">Aby uzyskać więcej informacji na temat tych typów wyników, zobacz [wyniki akcji w interfejsie Web API 2](../getting-started-with-aspnet-web-api/action-results.md).</span><span class="sxs-lookup"><span data-stu-id="18147-127">For more information about these result types, see [Action Results in Web Api 2](../getting-started-with-aspnet-web-api/action-results.md).</span></span>

## <a name="testing-actions-that-return-httpresponsemessage"></a><span data-ttu-id="18147-128">Akcje testowania zwracające HttpResponseMessage</span><span class="sxs-lookup"><span data-stu-id="18147-128">Testing Actions that Return HttpResponseMessage</span></span>

<span data-ttu-id="18147-129">Oto przykład kontrolera, którego akcje zwracają **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="18147-129">Here is an example of a controller whose actions return **HttpResponseMessage**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

<span data-ttu-id="18147-130">Zwróć uwagę na to, że kontroler używa iniekcji zależności, aby wstrzyknąć `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="18147-130">Notice the controller uses dependency injection to inject an `IProductRepository`.</span></span> <span data-ttu-id="18147-131">Sprawia, że kontroler jest bardziej weryfikowalne, ponieważ można wstrzyknąć repozytorium makiety.</span><span class="sxs-lookup"><span data-stu-id="18147-131">That makes the controller more testable, because you can inject a mock repository.</span></span> <span data-ttu-id="18147-132">Poniższy test jednostkowy sprawdza, czy metoda `Get` zapisuje `Product` do treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="18147-132">The following unit test verifies that the `Get` method writes a `Product` to the response body.</span></span> <span data-ttu-id="18147-133">Załóżmy, że `repository` jest `IProductRepository`a.</span><span class="sxs-lookup"><span data-stu-id="18147-133">Assume that `repository` is a mock `IProductRepository`.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

<span data-ttu-id="18147-134">Ważne jest, aby ustawić **żądanie** i **konfigurację** na kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="18147-134">It's important to set **Request** and **Configuration** on the controller.</span></span> <span data-ttu-id="18147-135">W przeciwnym razie test zakończy się niepowodzeniem z **ArgumentNullException** lub **InvalidOperationException**.</span><span class="sxs-lookup"><span data-stu-id="18147-135">Otherwise, the test will fail with an **ArgumentNullException** or **InvalidOperationException**.</span></span>

## <a name="testing-link-generation"></a><span data-ttu-id="18147-136">Testowanie generacji linków</span><span class="sxs-lookup"><span data-stu-id="18147-136">Testing Link Generation</span></span>

<span data-ttu-id="18147-137">Metoda `Post` wywołuje **UrlHelper. link** , aby utworzyć linki w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="18147-137">The `Post` method calls **UrlHelper.Link** to create links in the response.</span></span> <span data-ttu-id="18147-138">W teście jednostkowym wymagana jest nieco większa konfiguracja:</span><span class="sxs-lookup"><span data-stu-id="18147-138">This requires a little more setup in the unit test:</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

<span data-ttu-id="18147-139">Klasa **UrlHelper** wymaga adresu URL żądania i danych tras, dlatego test musi określić wartości.</span><span class="sxs-lookup"><span data-stu-id="18147-139">The **UrlHelper** class needs the request URL and route data, so the test has to set values for these.</span></span> <span data-ttu-id="18147-140">Inną opcją jest makieta lub szczątkowa **UrlHelper**.</span><span class="sxs-lookup"><span data-stu-id="18147-140">Another option is mock or stub **UrlHelper**.</span></span> <span data-ttu-id="18147-141">W tym podejściu zastąpisz wartość domyślną [ApiController. URL](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) z wersją makiety lub szczątkową zwracającą wartość stałą.</span><span class="sxs-lookup"><span data-stu-id="18147-141">With this approach, you replace the default value of [ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) with a mock or stub version that returns a fixed value.</span></span>

<span data-ttu-id="18147-142">Ponownie napiszemy test przy użyciu struktury [MOQ](https://github.com/Moq) .</span><span class="sxs-lookup"><span data-stu-id="18147-142">Let's rewrite the test using the [Moq](https://github.com/Moq) framework.</span></span> <span data-ttu-id="18147-143">Zainstaluj pakiet NuGet `Moq` w projekcie testowym.</span><span class="sxs-lookup"><span data-stu-id="18147-143">Install the `Moq` NuGet package in the test project.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

<span data-ttu-id="18147-144">W tej wersji nie trzeba konfigurować żadnych danych tras, ponieważ **UrlHelper** makiety zwraca ciąg stały.</span><span class="sxs-lookup"><span data-stu-id="18147-144">In this version, you don't need to set up any route data, because the mock **UrlHelper** returns a constant string.</span></span>

## <a name="testing-actions-that-return-ihttpactionresult"></a><span data-ttu-id="18147-145">Akcje testowania zwracające IHttpActionResult</span><span class="sxs-lookup"><span data-stu-id="18147-145">Testing Actions that Return IHttpActionResult</span></span>

<span data-ttu-id="18147-146">W interfejsie Web API 2 akcja kontrolera może zwracać **IHttpActionResult**, która jest analogiczna do **ACTIONRESULT** w ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="18147-146">In Web API 2, a controller action can return **IHttpActionResult**, which is analogous to **ActionResult** in ASP.NET MVC.</span></span> <span data-ttu-id="18147-147">Interfejs **IHttpActionResult** definiuje wzorzec poleceń służący do tworzenia odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="18147-147">The **IHttpActionResult** interface defines a command pattern for creating HTTP responses.</span></span> <span data-ttu-id="18147-148">Zamiast bezpośredniego tworzenia odpowiedzi kontroler zwraca **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="18147-148">Instead of creating the response directly, the controller returns an **IHttpActionResult**.</span></span> <span data-ttu-id="18147-149">Później potok wywoła **IHttpActionResult** do utworzenia odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="18147-149">Later, the pipeline invokes the **IHttpActionResult** to create the response.</span></span> <span data-ttu-id="18147-150">Takie podejście ułatwia pisanie testów jednostkowych, ponieważ można pominąć wiele konfiguracji, które są potrzebne do **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="18147-150">This approach makes it easier to write unit tests, because you can skip a lot of the setup that is needed for **HttpResponseMessage**.</span></span>

<span data-ttu-id="18147-151">Oto przykładowy kontroler, którego akcje zwracają **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="18147-151">Here is an example controller whose actions return **IHttpActionResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

<span data-ttu-id="18147-152">Ten przykład pokazuje niektóre typowe wzorce przy użyciu **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="18147-152">This example shows some common patterns using **IHttpActionResult**.</span></span> <span data-ttu-id="18147-153">Zobaczmy, jak przeprowadzić testy jednostkowe.</span><span class="sxs-lookup"><span data-stu-id="18147-153">Let's see how to unit test them.</span></span>

### <a name="action-returns-200-ok-with-a-response-body"></a><span data-ttu-id="18147-154">Akcja zwraca 200 (OK) z treścią odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="18147-154">Action returns 200 (OK) with a response body</span></span>

<span data-ttu-id="18147-155">Metoda `Get` wywołuje `Ok(product)`, jeśli produkt zostanie znaleziony.</span><span class="sxs-lookup"><span data-stu-id="18147-155">The `Get` method calls `Ok(product)` if the product is found.</span></span> <span data-ttu-id="18147-156">W teście jednostkowym upewnij się, że zwracany typ to **OkNegotiatedContentResult** , a zwrócony produkt ma prawidłowy identyfikator.</span><span class="sxs-lookup"><span data-stu-id="18147-156">In the unit test, make sure the return type is **OkNegotiatedContentResult** and the returned product has the right ID.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

<span data-ttu-id="18147-157">Zwróć uwagę, że test jednostkowy nie wykonuje wyniku akcji.</span><span class="sxs-lookup"><span data-stu-id="18147-157">Notice that the unit test doesn't execute the action result.</span></span> <span data-ttu-id="18147-158">Można założyć, że wyniki akcji powodują poprawne utworzenie odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="18147-158">You can assume the action result creates the HTTP response correctly.</span></span> <span data-ttu-id="18147-159">(Dlatego, że struktura internetowego interfejsu API ma własne testy jednostkowe!)</span><span class="sxs-lookup"><span data-stu-id="18147-159">(That's why the Web API framework has its own unit tests!)</span></span>

### <a name="action-returns-404-not-found"></a><span data-ttu-id="18147-160">Akcja zwraca 404 (nie znaleziono)</span><span class="sxs-lookup"><span data-stu-id="18147-160">Action returns 404 (Not Found)</span></span>

<span data-ttu-id="18147-161">Metoda `Get` wywołuje `NotFound()`, jeśli produkt nie zostanie znaleziony.</span><span class="sxs-lookup"><span data-stu-id="18147-161">The `Get` method calls `NotFound()` if the product is not found.</span></span> <span data-ttu-id="18147-162">W takim przypadku test jednostkowy sprawdza tylko, czy typem zwracanym jest **NotFoundResult**.</span><span class="sxs-lookup"><span data-stu-id="18147-162">For this case, the unit test just checks if the return type is **NotFoundResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a><span data-ttu-id="18147-163">Akcja zwraca 200 (OK) bez treści odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="18147-163">Action returns 200 (OK) with no response body</span></span>

<span data-ttu-id="18147-164">Metoda `Delete` wywołuje `Ok()` do zwrócenia pustej odpowiedzi HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="18147-164">The `Delete` method calls `Ok()` to return an empty HTTP 200 response.</span></span> <span data-ttu-id="18147-165">Podobnie jak w poprzednim przykładzie, test jednostkowy sprawdza zwracany typ, w tym przypadku **OkResult**.</span><span class="sxs-lookup"><span data-stu-id="18147-165">Like the previous example, the unit test checks the return type, in this case **OkResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a><span data-ttu-id="18147-166">Akcja zwraca 201 (utworzono) z nagłówkiem lokalizacji</span><span class="sxs-lookup"><span data-stu-id="18147-166">Action returns 201 (Created) with a Location header</span></span>

<span data-ttu-id="18147-167">Metoda `Post` wywołuje `CreatedAtRoute`, aby zwrócić odpowiedź HTTP 201 z identyfikatorem URI w nagłówku lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="18147-167">The `Post` method calls `CreatedAtRoute` to return an HTTP 201 response with a URI in the Location header.</span></span> <span data-ttu-id="18147-168">W teście jednostkowym Sprawdź, czy akcja ustawia poprawne wartości routingu.</span><span class="sxs-lookup"><span data-stu-id="18147-168">In the unit test, verify that the action sets the correct routing values.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a><span data-ttu-id="18147-169">Akcja zwraca kolejną 2xx z treścią odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="18147-169">Action returns another 2xx with a response body</span></span>

<span data-ttu-id="18147-170">Metoda `Put` wywołuje `Content` do zwrócenia odpowiedzi HTTP 202 (zaakceptowane) z treścią odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="18147-170">The `Put` method calls `Content` to return an HTTP 202 (Accepted) response with a response body.</span></span> <span data-ttu-id="18147-171">Ten przypadek jest podobny do zwraca 200 (OK), ale test jednostkowy powinien również sprawdzić kod stanu.</span><span class="sxs-lookup"><span data-stu-id="18147-171">This case is similar to returning 200 (OK), but the unit test should also check the status code.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a><span data-ttu-id="18147-172">Dodatkowe materiały</span><span class="sxs-lookup"><span data-stu-id="18147-172">Additional Resources</span></span>

- [<span data-ttu-id="18147-173">Imitacja Entity Framework, gdy testy jednostkowe ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="18147-173">Mocking Entity Framework when Unit Testing ASP.NET Web API 2</span></span>](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- <span data-ttu-id="18147-174">[Pisanie testów dla usługi internetowego interfejsu API ASP.NET](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (wpis w blogu przez Youssef Moussaoui).</span><span class="sxs-lookup"><span data-stu-id="18147-174">[Writing tests for an ASP.NET Web API service](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (blog post by Youssef Moussaoui).</span></span>
- [<span data-ttu-id="18147-175">Debugowanie interfejsu API sieci Web ASP.NET z debugerem tras</span><span class="sxs-lookup"><span data-stu-id="18147-175">Debugging ASP.NET Web API with Route Debugger</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
