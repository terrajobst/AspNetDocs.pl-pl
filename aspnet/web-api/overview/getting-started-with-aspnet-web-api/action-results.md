---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: Akcja wyniki w interfejsie Web API 2 — ASP.NET 4.x
author: MikeWasson
description: W tym artykule opisano, jak ASP.NET Web API konwertuje wartość zwracaną akcji kontrolera, do komunikatu odpowiedzi HTTP w programie ASP.NET 4.x.
ms.author: riande
ms.date: 02/03/2014
ms.custom: seoapril2019
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: 87f71938a5c5f38d3a456ba9339540f67e236e1a
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59400896"
---
# <a name="action-results-in-web-api-2"></a><span data-ttu-id="263fc-103">Wyniki akcji we wzorcu Web API 2</span><span class="sxs-lookup"><span data-stu-id="263fc-103">Action Results in Web API 2</span></span>

<span data-ttu-id="263fc-104">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="263fc-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="263fc-105">W tym temacie opisano, jak ASP.NET Web API konwertuje wartość zwracaną akcji kontrolera, do komunikatu odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="263fc-105">This topic describes how ASP.NET Web API converts the return value from a controller action into an HTTP response message.</span></span>

<span data-ttu-id="263fc-106">Akcja kontrolera interfejsu API sieci Web może zwracać dowolne z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="263fc-106">A Web API controller action can return any of the following:</span></span>

1. <span data-ttu-id="263fc-107">void</span><span class="sxs-lookup"><span data-stu-id="263fc-107">void</span></span>
2. <span data-ttu-id="263fc-108">**HttpResponseMessage**</span><span class="sxs-lookup"><span data-stu-id="263fc-108">**HttpResponseMessage**</span></span>
3. <span data-ttu-id="263fc-109">**IHttpActionResult**</span><span class="sxs-lookup"><span data-stu-id="263fc-109">**IHttpActionResult**</span></span>
4. <span data-ttu-id="263fc-110">Innego typu</span><span class="sxs-lookup"><span data-stu-id="263fc-110">Some other type</span></span>

<span data-ttu-id="263fc-111">W zależności od tego, który z nich jest zwracany, interfejsu API sieci Web używa innego mechanizmu do tworzenia odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="263fc-111">Depending on which of these is returned, Web API uses a different mechanism to create the HTTP response.</span></span>

| <span data-ttu-id="263fc-112">Zwracany typ</span><span class="sxs-lookup"><span data-stu-id="263fc-112">Return type</span></span> | <span data-ttu-id="263fc-113">Jak interfejs API sieci Web tworzy odpowiedź</span><span class="sxs-lookup"><span data-stu-id="263fc-113">How Web API creates the response</span></span> |
| --- | --- |
| <span data-ttu-id="263fc-114">void</span><span class="sxs-lookup"><span data-stu-id="263fc-114">void</span></span> | <span data-ttu-id="263fc-115">Zwraca pusty 204 (Brak zawartości)</span><span class="sxs-lookup"><span data-stu-id="263fc-115">Return empty 204 (No Content)</span></span> |
| <span data-ttu-id="263fc-116">**HttpResponseMessage**</span><span class="sxs-lookup"><span data-stu-id="263fc-116">**HttpResponseMessage**</span></span> | <span data-ttu-id="263fc-117">Konwertuj bezpośrednio do komunikatu odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="263fc-117">Convert directly to an HTTP response message.</span></span> |
| <span data-ttu-id="263fc-118">**IHttpActionResult**</span><span class="sxs-lookup"><span data-stu-id="263fc-118">**IHttpActionResult**</span></span> | <span data-ttu-id="263fc-119">Wywołaj **ExecuteAsync** utworzyć **obiektu HttpResponseMessage**, następnie przekonwertować komunikatu odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="263fc-119">Call **ExecuteAsync** to create an **HttpResponseMessage**, then convert to an HTTP response message.</span></span> |
| <span data-ttu-id="263fc-120">Inny typ</span><span class="sxs-lookup"><span data-stu-id="263fc-120">Other type</span></span> | <span data-ttu-id="263fc-121">Zapisz wartość zwracaną serializacji do treści odpowiedzi; Zwraca 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="263fc-121">Write the serialized return value into the response body; return 200 (OK).</span></span> |

<span data-ttu-id="263fc-122">Pozostała część tego tematu opisano wszystkie opcje, które bardziej szczegółowo.</span><span class="sxs-lookup"><span data-stu-id="263fc-122">The rest of this topic describes each option in more detail.</span></span>

## <a name="void"></a><span data-ttu-id="263fc-123">void</span><span class="sxs-lookup"><span data-stu-id="263fc-123">void</span></span>

<span data-ttu-id="263fc-124">Jeśli typ zwracany jest `void`, interfejs API sieci Web po prostu zwraca pustą odpowiedź HTTP z kodem stanu 204 (Brak zawartości).</span><span class="sxs-lookup"><span data-stu-id="263fc-124">If the return type is `void`, Web API simply returns an empty HTTP response with status code 204 (No Content).</span></span>

<span data-ttu-id="263fc-125">Przykład kontrolera:</span><span class="sxs-lookup"><span data-stu-id="263fc-125">Example controller:</span></span>

[!code-csharp[Main](action-results/samples/sample1.cs)]

<span data-ttu-id="263fc-126">Odpowiedź HTTP:</span><span class="sxs-lookup"><span data-stu-id="263fc-126">HTTP response:</span></span>

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a><span data-ttu-id="263fc-127">HttpResponseMessage</span><span class="sxs-lookup"><span data-stu-id="263fc-127">HttpResponseMessage</span></span>

<span data-ttu-id="263fc-128">Jeśli działanie zwróci [obiektu HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), internetowy interfejs API konwertuje wartość zwracaną bezpośrednio na komunikat odpowiedzi HTTP przy użyciu właściwości **obiektu HttpResponseMessage** obiekt do wypełnienia odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="263fc-128">If the action returns an [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), Web API converts the return value directly into an HTTP response message, using the properties of the **HttpResponseMessage** object to populate the response.</span></span>

<span data-ttu-id="263fc-129">Ta opcja zapewnia wysoki poziom kontroli nad komunikat odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="263fc-129">This option gives you a lot of control over the response message.</span></span> <span data-ttu-id="263fc-130">Na przykład następującej akcji kontrolera ustawia nagłówek Cache-Control.</span><span class="sxs-lookup"><span data-stu-id="263fc-130">For example, the following controller action sets the Cache-Control header.</span></span>

[!code-csharp[Main](action-results/samples/sample3.cs)]

<span data-ttu-id="263fc-131">Response:</span><span class="sxs-lookup"><span data-stu-id="263fc-131">Response:</span></span>

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

<span data-ttu-id="263fc-132">W przypadku przekazania model domeny, aby **CreateResponse** metody, interfejs API sieci Web używa [nośnika elementu formatującego](../formats-and-model-binding/media-formatters.md) do zapisania Zserializowany model na treść odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="263fc-132">If you pass a domain model to the **CreateResponse** method, Web API uses a [media formatter](../formats-and-model-binding/media-formatters.md) to write the serialized model into the response body.</span></span>

[!code-csharp[Main](action-results/samples/sample5.cs)]

<span data-ttu-id="263fc-133">Internetowy interfejs API używa nagłówek Accept w żądaniu, aby wybrać element formatujący.</span><span class="sxs-lookup"><span data-stu-id="263fc-133">Web API uses the Accept header in the request to choose the formatter.</span></span> <span data-ttu-id="263fc-134">Aby uzyskać więcej informacji, zobacz [negocjacje zawartości](../formats-and-model-binding/content-negotiation.md).</span><span class="sxs-lookup"><span data-stu-id="263fc-134">For more information, see [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span></span>

## <a name="ihttpactionresult"></a><span data-ttu-id="263fc-135">IHttpActionResult</span><span class="sxs-lookup"><span data-stu-id="263fc-135">IHttpActionResult</span></span>

<span data-ttu-id="263fc-136">**IHttpActionResult** interfejsu została wprowadzona w sieci Web API 2.</span><span class="sxs-lookup"><span data-stu-id="263fc-136">The **IHttpActionResult** interface was introduced in Web API 2.</span></span> <span data-ttu-id="263fc-137">Zasadniczo definiuje **obiektu HttpResponseMessage** fabryki.</span><span class="sxs-lookup"><span data-stu-id="263fc-137">Essentially, it defines an **HttpResponseMessage** factory.</span></span> <span data-ttu-id="263fc-138">Oto niektóre korzyści wynikające z używania **IHttpActionResult** interfejsu:</span><span class="sxs-lookup"><span data-stu-id="263fc-138">Here are some advantages of using the **IHttpActionResult** interface:</span></span>

- <span data-ttu-id="263fc-139">Upraszcza [testy jednostkowe](../testing-and-debugging/unit-testing-controllers-in-web-api.md) kontrolerach.</span><span class="sxs-lookup"><span data-stu-id="263fc-139">Simplifies [unit testing](../testing-and-debugging/unit-testing-controllers-in-web-api.md) your controllers.</span></span>
- <span data-ttu-id="263fc-140">Przenosi wspólnej logiki do tworzenia odpowiedzi HTTP do osobnych klas.</span><span class="sxs-lookup"><span data-stu-id="263fc-140">Moves common logic for creating HTTP responses into separate classes.</span></span>
- <span data-ttu-id="263fc-141">Sprawia, że celem akcji kontrolera, bardziej zrozumiały, ukrywając niskopoziomowych szczegółów konstruowanie odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="263fc-141">Makes the intent of the controller action clearer, by hiding the low-level details of constructing the response.</span></span>

<span data-ttu-id="263fc-142">**IHttpActionResult** zawiera jedną metodę **ExecuteAsync**, które asynchronicznie tworzy **obiektu HttpResponseMessage** wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="263fc-142">**IHttpActionResult** contains a single method, **ExecuteAsync**, which asynchronously creates an **HttpResponseMessage** instance.</span></span>

[!code-csharp[Main](action-results/samples/sample6.cs)]

<span data-ttu-id="263fc-143">Jeśli akcja kontrolera zwraca **IHttpActionResult**, wywołań interfejsu API sieci Web **ExecuteAsync** metodę w celu utworzenia **obiektu HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="263fc-143">If a controller action returns an **IHttpActionResult**, Web API calls the **ExecuteAsync** method to create an **HttpResponseMessage**.</span></span> <span data-ttu-id="263fc-144">A następnie konwertuje **obiektu HttpResponseMessage** do komunikatu odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="263fc-144">Then it converts the **HttpResponseMessage** into an HTTP response message.</span></span>

<span data-ttu-id="263fc-145">Poniżej przedstawiono proste wdrażanie **IHttpActionResult** tworząca odpowiedź jako zwykły tekst:</span><span class="sxs-lookup"><span data-stu-id="263fc-145">Here is a simple implementation of **IHttpActionResult** that creates a plain text response:</span></span>

[!code-csharp[Main](action-results/samples/sample7.cs)]

<span data-ttu-id="263fc-146">Przykład akcji kontrolera:</span><span class="sxs-lookup"><span data-stu-id="263fc-146">Example controller action:</span></span>

[!code-csharp[Main](action-results/samples/sample8.cs)]

<span data-ttu-id="263fc-147">Response:</span><span class="sxs-lookup"><span data-stu-id="263fc-147">Response:</span></span>

[!code-console[Main](action-results/samples/sample9.cmd)]

<span data-ttu-id="263fc-148">Częściej, użyjesz **IHttpActionResult** zdefiniowane w implementacji **[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="263fc-148">More often, you will use the **IHttpActionResult** implementations defined in the **[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** namespace.</span></span> <span data-ttu-id="263fc-149">**Klasy ApiController** klasa definiuje metody pomocnika, które zwracają wyniki tych wbudowanych akcji.</span><span class="sxs-lookup"><span data-stu-id="263fc-149">The **ApiController** class defines helper methods that return these built-in action results.</span></span>

<span data-ttu-id="263fc-150">W poniższym przykładzie, jeśli żądanie nie jest zgodny z istniejący identyfikator produktu, kontroler wywołuje [ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) do tworzenia odpowiedzi 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="263fc-150">In the following example, if the request does not match an existing product ID, the controller calls [ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) to create a 404 (Not Found) response.</span></span> <span data-ttu-id="263fc-151">W przeciwnym razie kontroler wywołuje [ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx), która tworzy odpowiedź 200 (OK), zawiera produktu.</span><span class="sxs-lookup"><span data-stu-id="263fc-151">Otherwise, the controller calls [ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx), which creates a 200 (OK) response that contains the product.</span></span>

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a><span data-ttu-id="263fc-152">Inne typy zwracane</span><span class="sxs-lookup"><span data-stu-id="263fc-152">Other Return Types</span></span>

<span data-ttu-id="263fc-153">Dla wszystkich innych typów zwrotu, korzysta z interfejsu API sieci Web [nośnika elementu formatującego](../formats-and-model-binding/media-formatters.md) serializować wartość zwracaną.</span><span class="sxs-lookup"><span data-stu-id="263fc-153">For all other return types, Web API uses a [media formatter](../formats-and-model-binding/media-formatters.md) to serialize the return value.</span></span> <span data-ttu-id="263fc-154">Interfejs API sieci Web zapisuje wartość zserializowana do treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="263fc-154">Web API writes the serialized value into the response body.</span></span> <span data-ttu-id="263fc-155">Kod stanu odpowiedzi to 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="263fc-155">The response status code is 200 (OK).</span></span>

[!code-csharp[Main](action-results/samples/sample11.cs)]

<span data-ttu-id="263fc-156">Wadą tego podejścia jest, że bezpośrednio nie może zwrócić kod błędu, takie jak 404.</span><span class="sxs-lookup"><span data-stu-id="263fc-156">A disadvantage of this approach is that you cannot directly return an error code, such as 404.</span></span> <span data-ttu-id="263fc-157">Jednak może zgłosić **HttpResponseException** dla kodów błędów.</span><span class="sxs-lookup"><span data-stu-id="263fc-157">However, you can throw an **HttpResponseException** for error codes.</span></span> <span data-ttu-id="263fc-158">Aby uzyskać więcej informacji, zobacz [Exception Handling in Web API platformy ASP.NET](../error-handling/exception-handling.md).</span><span class="sxs-lookup"><span data-stu-id="263fc-158">For more information, see [Exception Handling in ASP.NET Web API](../error-handling/exception-handling.md).</span></span>

<span data-ttu-id="263fc-159">Internetowy interfejs API używa nagłówek Accept w żądaniu, aby wybrać element formatujący.</span><span class="sxs-lookup"><span data-stu-id="263fc-159">Web API uses the Accept header in the request to choose the formatter.</span></span> <span data-ttu-id="263fc-160">Aby uzyskać więcej informacji, zobacz [negocjacje zawartości](../formats-and-model-binding/content-negotiation.md).</span><span class="sxs-lookup"><span data-stu-id="263fc-160">For more information, see [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span></span>

<span data-ttu-id="263fc-161">Przykładowe żądanie</span><span class="sxs-lookup"><span data-stu-id="263fc-161">Example request</span></span>

[!code-console[Main](action-results/samples/sample12.cmd)]

<span data-ttu-id="263fc-162">Przykładowa odpowiedź:</span><span class="sxs-lookup"><span data-stu-id="263fc-162">Example response:</span></span>

[!code-console[Main](action-results/samples/sample13.cmd)]
