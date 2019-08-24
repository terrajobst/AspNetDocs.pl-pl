---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: Wyniki akcji w interfejsie Web API 2 — ASP.NET 4. x
author: MikeWasson
description: Opisuje sposób, w jaki interfejs API sieci Web ASP.NET konwertuje wartość zwracaną z akcji kontrolera na komunikat odpowiedzi HTTP w ASP.NET 4. x.
ms.author: riande
ms.date: 02/03/2014
ms.custom: seoapril2019
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: 1eaaf8e87168096683212fa66d3ddf415ad6b22b
ms.sourcegitcommit: b95316530fa51087d6c400ff91814fe37e73f7e8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/23/2019
ms.locfileid: "70000717"
---
# <a name="action-results-in-web-api-2"></a><span data-ttu-id="d2daf-103">Wyniki akcji we wzorcu Web API 2</span><span class="sxs-lookup"><span data-stu-id="d2daf-103">Action Results in Web API 2</span></span>

<span data-ttu-id="d2daf-104">W tym temacie opisano sposób, w jaki interfejs API sieci Web ASP.NET konwertuje wartość zwracaną z akcji kontrolera na komunikat odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="d2daf-104">This topic describes how ASP.NET Web API converts the return value from a controller action into an HTTP response message.</span></span>

<span data-ttu-id="d2daf-105">Akcja kontrolera interfejsu API sieci Web może zwracać dowolne z następujących elementów:</span><span class="sxs-lookup"><span data-stu-id="d2daf-105">A Web API controller action can return any of the following:</span></span>

1. <span data-ttu-id="d2daf-106">void</span><span class="sxs-lookup"><span data-stu-id="d2daf-106">void</span></span>
2. <span data-ttu-id="d2daf-107">**HttpResponseMessage**</span><span class="sxs-lookup"><span data-stu-id="d2daf-107">**HttpResponseMessage**</span></span>
3. <span data-ttu-id="d2daf-108">**IHttpActionResult**</span><span class="sxs-lookup"><span data-stu-id="d2daf-108">**IHttpActionResult**</span></span>
4. <span data-ttu-id="d2daf-109">Inny typ</span><span class="sxs-lookup"><span data-stu-id="d2daf-109">Some other type</span></span>

<span data-ttu-id="d2daf-110">W zależności od tego, które z nich są zwracane, interfejs API sieci Web używa innego mechanizmu do tworzenia odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="d2daf-110">Depending on which of these is returned, Web API uses a different mechanism to create the HTTP response.</span></span>

| <span data-ttu-id="d2daf-111">Typ zwracany</span><span class="sxs-lookup"><span data-stu-id="d2daf-111">Return type</span></span> | <span data-ttu-id="d2daf-112">Jak interfejs API sieci Web tworzy odpowiedź</span><span class="sxs-lookup"><span data-stu-id="d2daf-112">How Web API creates the response</span></span> |
| --- | --- |
| <span data-ttu-id="d2daf-113">void</span><span class="sxs-lookup"><span data-stu-id="d2daf-113">void</span></span> | <span data-ttu-id="d2daf-114">Zwróć pustą 204 (brak zawartości)</span><span class="sxs-lookup"><span data-stu-id="d2daf-114">Return empty 204 (No Content)</span></span> |
| <span data-ttu-id="d2daf-115">**HttpResponseMessage**</span><span class="sxs-lookup"><span data-stu-id="d2daf-115">**HttpResponseMessage**</span></span> | <span data-ttu-id="d2daf-116">Konwertuj bezpośrednio na komunikat odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="d2daf-116">Convert directly to an HTTP response message.</span></span> |
| <span data-ttu-id="d2daf-117">**IHttpActionResult**</span><span class="sxs-lookup"><span data-stu-id="d2daf-117">**IHttpActionResult**</span></span> | <span data-ttu-id="d2daf-118">Wywołaj **wywoływanie ExecuteAsync** , aby utworzyć **HttpResponseMessage**, a następnie przekonwertuj na komunikat odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="d2daf-118">Call **ExecuteAsync** to create an **HttpResponseMessage**, then convert to an HTTP response message.</span></span> |
| <span data-ttu-id="d2daf-119">Inny typ</span><span class="sxs-lookup"><span data-stu-id="d2daf-119">Other type</span></span> | <span data-ttu-id="d2daf-120">Zapisz zserializowaną wartość zwrotną w treści odpowiedzi. Zwróć 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="d2daf-120">Write the serialized return value into the response body; return 200 (OK).</span></span> |

<span data-ttu-id="d2daf-121">W pozostałej części tego tematu opisano każdą opcję bardziej szczegółowo.</span><span class="sxs-lookup"><span data-stu-id="d2daf-121">The rest of this topic describes each option in more detail.</span></span>

## <a name="void"></a><span data-ttu-id="d2daf-122">void</span><span class="sxs-lookup"><span data-stu-id="d2daf-122">void</span></span>

<span data-ttu-id="d2daf-123">Jeśli zwracanym typem jest `void`, interfejs API sieci Web po prostu zwraca pustą odpowiedź HTTP z kodem stanu 204 (brak zawartości).</span><span class="sxs-lookup"><span data-stu-id="d2daf-123">If the return type is `void`, Web API simply returns an empty HTTP response with status code 204 (No Content).</span></span>

<span data-ttu-id="d2daf-124">Przykładowy kontroler:</span><span class="sxs-lookup"><span data-stu-id="d2daf-124">Example controller:</span></span>

[!code-csharp[Main](action-results/samples/sample1.cs)]

<span data-ttu-id="d2daf-125">Odpowiedź HTTP:</span><span class="sxs-lookup"><span data-stu-id="d2daf-125">HTTP response:</span></span>

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a><span data-ttu-id="d2daf-126">HttpResponseMessage</span><span class="sxs-lookup"><span data-stu-id="d2daf-126">HttpResponseMessage</span></span>

<span data-ttu-id="d2daf-127">Jeśli akcja zwraca [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), internetowy interfejs API konwertuje wartość zwracaną bezpośrednio na komunikat odpowiedzi HTTP, używając właściwości obiektu **HttpResponseMessage** do wypełnienia odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="d2daf-127">If the action returns an [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), Web API converts the return value directly into an HTTP response message, using the properties of the **HttpResponseMessage** object to populate the response.</span></span>

<span data-ttu-id="d2daf-128">Ta opcja zapewnia dużą kontrolę nad komunikatem odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="d2daf-128">This option gives you a lot of control over the response message.</span></span> <span data-ttu-id="d2daf-129">Na przykład następująca akcja kontrolera ustawia nagłówek Cache-Control.</span><span class="sxs-lookup"><span data-stu-id="d2daf-129">For example, the following controller action sets the Cache-Control header.</span></span>

[!code-csharp[Main](action-results/samples/sample3.cs)]

<span data-ttu-id="d2daf-130">Reakcji</span><span class="sxs-lookup"><span data-stu-id="d2daf-130">Response:</span></span>

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

<span data-ttu-id="d2daf-131">W przypadku przekazania modelu domeny do metody " **Xmlresponse** " interfejs API sieci Web używa programu [formatującego multimedia](../formats-and-model-binding/media-formatters.md) , aby napisać serializowany model do treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="d2daf-131">If you pass a domain model to the **CreateResponse** method, Web API uses a [media formatter](../formats-and-model-binding/media-formatters.md) to write the serialized model into the response body.</span></span>

[!code-csharp[Main](action-results/samples/sample5.cs)]

<span data-ttu-id="d2daf-132">Interfejs API sieci Web używa nagłówka Accept w żądaniu do wybrania programu formatującego.</span><span class="sxs-lookup"><span data-stu-id="d2daf-132">Web API uses the Accept header in the request to choose the formatter.</span></span> <span data-ttu-id="d2daf-133">Aby uzyskać więcej informacji, zobacz [negocjacje zawartości](../formats-and-model-binding/content-negotiation.md).</span><span class="sxs-lookup"><span data-stu-id="d2daf-133">For more information, see [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span></span>

## <a name="ihttpactionresult"></a><span data-ttu-id="d2daf-134">IHttpActionResult</span><span class="sxs-lookup"><span data-stu-id="d2daf-134">IHttpActionResult</span></span>

<span data-ttu-id="d2daf-135">Interfejs **IHttpActionResult** został wprowadzony w interfejsie Web API 2.</span><span class="sxs-lookup"><span data-stu-id="d2daf-135">The **IHttpActionResult** interface was introduced in Web API 2.</span></span> <span data-ttu-id="d2daf-136">Zasadniczo definiuje fabrykę **HttpResponseMessage** .</span><span class="sxs-lookup"><span data-stu-id="d2daf-136">Essentially, it defines an **HttpResponseMessage** factory.</span></span> <span data-ttu-id="d2daf-137">Poniżej przedstawiono niektóre zalety korzystania z interfejsu **IHttpActionResult** :</span><span class="sxs-lookup"><span data-stu-id="d2daf-137">Here are some advantages of using the **IHttpActionResult** interface:</span></span>

- <span data-ttu-id="d2daf-138">Upraszcza [testowanie jednostkowe](../testing-and-debugging/unit-testing-controllers-in-web-api.md) kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="d2daf-138">Simplifies [unit testing](../testing-and-debugging/unit-testing-controllers-in-web-api.md) your controllers.</span></span>
- <span data-ttu-id="d2daf-139">Przenosi wspólną logikę do tworzenia odpowiedzi HTTP w oddzielnych klasach.</span><span class="sxs-lookup"><span data-stu-id="d2daf-139">Moves common logic for creating HTTP responses into separate classes.</span></span>
- <span data-ttu-id="d2daf-140">Sprawia, że zamiarowa akcja kontrolera jest przejrzysta, ukrywając szczegóły niskiego poziomu konstruowania odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="d2daf-140">Makes the intent of the controller action clearer, by hiding the low-level details of constructing the response.</span></span>

<span data-ttu-id="d2daf-141">**IHttpActionResult** zawiera jedną metodę **wywoływanie ExecuteAsync**, która asynchronicznie tworzy wystąpienie **HttpResponseMessage** .</span><span class="sxs-lookup"><span data-stu-id="d2daf-141">**IHttpActionResult** contains a single method, **ExecuteAsync**, which asynchronously creates an **HttpResponseMessage** instance.</span></span>

[!code-csharp[Main](action-results/samples/sample6.cs)]

<span data-ttu-id="d2daf-142">Jeśli akcja kontrolera zwraca **IHttpActionResult**, interfejs API sieci Web wywołuje metodę **wywoływanie ExecuteAsync** , aby utworzyć **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="d2daf-142">If a controller action returns an **IHttpActionResult**, Web API calls the **ExecuteAsync** method to create an **HttpResponseMessage**.</span></span> <span data-ttu-id="d2daf-143">Następnie konwertuje **HttpResponseMessage** na komunikat odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="d2daf-143">Then it converts the **HttpResponseMessage** into an HTTP response message.</span></span>

<span data-ttu-id="d2daf-144">Oto prosta implementacja **IHttpActionResult** , która tworzy odpowiedź w postaci zwykłego tekstu:</span><span class="sxs-lookup"><span data-stu-id="d2daf-144">Here is a simple implementation of **IHttpActionResult** that creates a plain text response:</span></span>

[!code-csharp[Main](action-results/samples/sample7.cs)]

<span data-ttu-id="d2daf-145">Przykładowa akcja kontrolera:</span><span class="sxs-lookup"><span data-stu-id="d2daf-145">Example controller action:</span></span>

[!code-csharp[Main](action-results/samples/sample8.cs)]

<span data-ttu-id="d2daf-146">Reakcji</span><span class="sxs-lookup"><span data-stu-id="d2daf-146">Response:</span></span>

[!code-console[Main](action-results/samples/sample9.cmd)]

<span data-ttu-id="d2daf-147">Często używane są implementacje **IHttpActionResult** zdefiniowane w przestrzeni nazw **[System. Web. http. results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="d2daf-147">More often, you use the **IHttpActionResult** implementations defined in the **[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** namespace.</span></span> <span data-ttu-id="d2daf-148">Klasa **ApiController** definiuje metody pomocnika, które zwracają te wbudowane wyniki akcji.</span><span class="sxs-lookup"><span data-stu-id="d2daf-148">The **ApiController** class defines helper methods that return these built-in action results.</span></span>

<span data-ttu-id="d2daf-149">W poniższym przykładzie, jeśli żądanie nie jest zgodne z istniejącym IDENTYFIKATORem produktu, kontroler wywoła [ApiController. NOTFOUND](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) , aby utworzyć odpowiedź 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="d2daf-149">In the following example, if the request does not match an existing product ID, the controller calls [ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) to create a 404 (Not Found) response.</span></span> <span data-ttu-id="d2daf-150">W przeciwnym razie kontroler wywoła [ApiController. ok](https://msdn.microsoft.com/library/dn314591.aspx), co spowoduje utworzenie odpowiedzi 200 (ok) zawierającej produkt.</span><span class="sxs-lookup"><span data-stu-id="d2daf-150">Otherwise, the controller calls [ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx), which creates a 200 (OK) response that contains the product.</span></span>

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a><span data-ttu-id="d2daf-151">Inne typy zwracane</span><span class="sxs-lookup"><span data-stu-id="d2daf-151">Other Return Types</span></span>

<span data-ttu-id="d2daf-152">Dla wszystkich innych typów zwracanych interfejs API sieci Web używa programu [formatującego multimedia](../formats-and-model-binding/media-formatters.md) do serializacji wartości zwracanej.</span><span class="sxs-lookup"><span data-stu-id="d2daf-152">For all other return types, Web API uses a [media formatter](../formats-and-model-binding/media-formatters.md) to serialize the return value.</span></span> <span data-ttu-id="d2daf-153">Interfejs API sieci Web zapisuje zserializowaną wartość w treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="d2daf-153">Web API writes the serialized value into the response body.</span></span> <span data-ttu-id="d2daf-154">Kod stanu odpowiedzi to 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="d2daf-154">The response status code is 200 (OK).</span></span>

[!code-csharp[Main](action-results/samples/sample11.cs)]

<span data-ttu-id="d2daf-155">Wadą tego podejścia jest to, że nie można bezpośrednio zwrócić kodu błędu, takiego jak 404.</span><span class="sxs-lookup"><span data-stu-id="d2daf-155">A disadvantage of this approach is that you cannot directly return an error code, such as 404.</span></span> <span data-ttu-id="d2daf-156">Można jednak zgłosić **HttpResponseException** dla kodów błędów.</span><span class="sxs-lookup"><span data-stu-id="d2daf-156">However, you can throw an **HttpResponseException** for error codes.</span></span> <span data-ttu-id="d2daf-157">Aby uzyskać więcej informacji, zobacz [Obsługa wyjątków w interfejsie API sieci Web ASP.NET](../error-handling/exception-handling.md).</span><span class="sxs-lookup"><span data-stu-id="d2daf-157">For more information, see [Exception Handling in ASP.NET Web API](../error-handling/exception-handling.md).</span></span>

<span data-ttu-id="d2daf-158">Interfejs API sieci Web używa nagłówka Accept w żądaniu do wybrania programu formatującego.</span><span class="sxs-lookup"><span data-stu-id="d2daf-158">Web API uses the Accept header in the request to choose the formatter.</span></span> <span data-ttu-id="d2daf-159">Aby uzyskać więcej informacji, zobacz [negocjacje zawartości](../formats-and-model-binding/content-negotiation.md).</span><span class="sxs-lookup"><span data-stu-id="d2daf-159">For more information, see [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span></span>

<span data-ttu-id="d2daf-160">Przykładowe żądanie</span><span class="sxs-lookup"><span data-stu-id="d2daf-160">Example request</span></span>

[!code-console[Main](action-results/samples/sample12.cmd)]

<span data-ttu-id="d2daf-161">Przykładowa odpowiedź</span><span class="sxs-lookup"><span data-stu-id="d2daf-161">Example response</span></span>

[!code-console[Main](action-results/samples/sample13.cmd)]
