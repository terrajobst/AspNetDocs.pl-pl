---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: 'Wysyłanie danych formularza HTML w interfejsie Web API ASP.NET: form-urlencoded Data-ASP.NET 4. x'
author: MikeWasson
description: W tym artykule przedstawiono sposób publikowania danych formularza urlencoded na kontrolerze interfejsu API sieci Web przy użyciu ASP.NET 4. x
ms.author: riande
ms.date: 06/15/2012
ms.custom: seoapril2019
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 7243069dbd8051b1374ed6e0112c273b8fe26f61
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557604"
---
# <a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a><span data-ttu-id="5e466-103">Wysyłanie danych formularza HTML w interfejsie Web API ASP.NET: form-urlencoded Data</span><span class="sxs-lookup"><span data-stu-id="5e466-103">Sending HTML Form Data in ASP.NET Web API: Form-urlencoded Data</span></span>

<span data-ttu-id="5e466-104">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="5e466-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

## <a name="part-1-form-urlencoded-data"></a><span data-ttu-id="5e466-105">Część 1: formularz-urlencoded danych</span><span class="sxs-lookup"><span data-stu-id="5e466-105">Part 1: Form-urlencoded Data</span></span>

<span data-ttu-id="5e466-106">W tym artykule przedstawiono sposób publikowania danych formularza urlencoded na kontrolerze interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="5e466-106">This article shows how to post form-urlencoded data to a Web API controller.</span></span>

- [<span data-ttu-id="5e466-107">Przegląd formularzy HTML</span><span class="sxs-lookup"><span data-stu-id="5e466-107">Overview of HTML Forms</span></span>](#overview_of_html_forms)
- [<span data-ttu-id="5e466-108">Wysyłanie typów złożonych</span><span class="sxs-lookup"><span data-stu-id="5e466-108">Sending Complex Types</span></span>](#sending_complex_types)
- [<span data-ttu-id="5e466-109">Wysyłanie danych formularza za pośrednictwem technologii AJAX</span><span class="sxs-lookup"><span data-stu-id="5e466-109">Sending Form Data via AJAX</span></span>](#sending_form_data_via_ajax)
- [<span data-ttu-id="5e466-110">Wysyłanie typów prostych</span><span class="sxs-lookup"><span data-stu-id="5e466-110">Sending Simple Types</span></span>](#sending_simple_types)

> [!NOTE]
> <span data-ttu-id="5e466-111">[Pobierz ukończony projekt](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).</span><span class="sxs-lookup"><span data-stu-id="5e466-111">[Download the completed project](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).</span></span>

<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a><span data-ttu-id="5e466-112">Przegląd formularzy HTML</span><span class="sxs-lookup"><span data-stu-id="5e466-112">Overview of HTML Forms</span></span>

<span data-ttu-id="5e466-113">Formularze HTML używają elementu GET lub POST do wysyłania danych do serwera.</span><span class="sxs-lookup"><span data-stu-id="5e466-113">HTML forms use either GET or POST to send data to the server.</span></span> <span data-ttu-id="5e466-114">Atrybut **Method** elementu **form** daje metodę http:</span><span class="sxs-lookup"><span data-stu-id="5e466-114">The **method** attribute of the **form** element gives the HTTP method:</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

<span data-ttu-id="5e466-115">Metoda domyślna to GET.</span><span class="sxs-lookup"><span data-stu-id="5e466-115">The default method is GET.</span></span> <span data-ttu-id="5e466-116">Jeśli formularz używa GET, dane formularza są kodowane w identyfikatorze URI jako ciąg zapytania.</span><span class="sxs-lookup"><span data-stu-id="5e466-116">If the form uses GET, the form data is encoded in the URI as a query string.</span></span> <span data-ttu-id="5e466-117">Jeśli formularz używa wpisu POST, dane formularza są umieszczane w treści żądania.</span><span class="sxs-lookup"><span data-stu-id="5e466-117">If the form uses POST, the form data is placed in the request body.</span></span> <span data-ttu-id="5e466-118">W przypadku opublikowanych danych atrybut **Enctype** określa format treści żądania:</span><span class="sxs-lookup"><span data-stu-id="5e466-118">For POSTed data, the **enctype** attribute specifies the format of the request body:</span></span>

| <span data-ttu-id="5e466-119">enctype</span><span class="sxs-lookup"><span data-stu-id="5e466-119">enctype</span></span> | <span data-ttu-id="5e466-120">Opis</span><span class="sxs-lookup"><span data-stu-id="5e466-120">Description</span></span> |
| --- | --- |
| <span data-ttu-id="5e466-121">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="5e466-121">application/x-www-form-urlencoded</span></span> | <span data-ttu-id="5e466-122">Dane formularza są kodowane jako pary nazwa/wartość, podobnie jak ciąg zapytania identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="5e466-122">Form data is encoded as name/value pairs, similar to a URI query string.</span></span> <span data-ttu-id="5e466-123">Jest to domyślny format dla wpisu POST.</span><span class="sxs-lookup"><span data-stu-id="5e466-123">This is the default format for POST.</span></span> |
| <span data-ttu-id="5e466-124">wieloczęściowe/formularz-dane</span><span class="sxs-lookup"><span data-stu-id="5e466-124">multipart/form-data</span></span> | <span data-ttu-id="5e466-125">Dane formularza są kodowane jako wieloczęściowy komunikat MIME.</span><span class="sxs-lookup"><span data-stu-id="5e466-125">Form data is encoded as a multipart MIME message.</span></span> <span data-ttu-id="5e466-126">Użyj tego formatu, jeśli przekazujesz plik na serwer.</span><span class="sxs-lookup"><span data-stu-id="5e466-126">Use this format if you are uploading a file to the server.</span></span> |

<span data-ttu-id="5e466-127">Część 1 tego artykułu wygląda w formacie x-www-form-urlencoded.</span><span class="sxs-lookup"><span data-stu-id="5e466-127">Part 1 of this article looks at x-www-form-urlencoded format.</span></span> <span data-ttu-id="5e466-128">[Część 2](sending-html-form-data-part-2.md) zawiera opis wieloczęściowego MIME.</span><span class="sxs-lookup"><span data-stu-id="5e466-128">[Part 2](sending-html-form-data-part-2.md) describes multipart MIME.</span></span>

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a><span data-ttu-id="5e466-129">Wysyłanie typów złożonych</span><span class="sxs-lookup"><span data-stu-id="5e466-129">Sending Complex Types</span></span>

<span data-ttu-id="5e466-130">Zwykle wysyłany jest typ złożony składający się z wartości pobranych z kilku kontrolek formularza.</span><span class="sxs-lookup"><span data-stu-id="5e466-130">Typically, you will send a complex type, composed of values taken from several form controls.</span></span> <span data-ttu-id="5e466-131">Rozważmy następujący model, który reprezentuje aktualizację stanu:</span><span class="sxs-lookup"><span data-stu-id="5e466-131">Consider the following model that represents a status update:</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

<span data-ttu-id="5e466-132">Oto kontroler interfejsu API sieci Web, który akceptuje obiekt `Update` za pośrednictwem wpisu POST.</span><span class="sxs-lookup"><span data-stu-id="5e466-132">Here is a Web API controller that accepts an `Update` object via POST.</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> <span data-ttu-id="5e466-133">Ten kontroler korzysta z [routingu opartego na akcjach](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), więc szablon trasy to &quot;API/{Controller}/{Action}/{id}&quot;.</span><span class="sxs-lookup"><span data-stu-id="5e466-133">This controller uses [action-based routing](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), so the route template is &quot;api/{controller}/{action}/{id}&quot;.</span></span> <span data-ttu-id="5e466-134">Klient będzie ogłaszał dane w &quot;/API/Updates/Complex&quot;.</span><span class="sxs-lookup"><span data-stu-id="5e466-134">The client will post the data to &quot;/api/updates/complex&quot;.</span></span>

<span data-ttu-id="5e466-135">Teraz Napiszmy formularz HTML dla użytkowników w celu przesłania aktualizacji stanu.</span><span class="sxs-lookup"><span data-stu-id="5e466-135">Now let's write an HTML form for users to submit a status update.</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

<span data-ttu-id="5e466-136">Zwróć uwagę, że atrybut **Action** w formularzu jest identyfikatorem URI naszej akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="5e466-136">Notice that the **action** attribute on the form is the URI of our controller action.</span></span> <span data-ttu-id="5e466-137">Oto formularz z wprowadzonymi wartościami:</span><span class="sxs-lookup"><span data-stu-id="5e466-137">Here is the form with some values entered in:</span></span>

![](sending-html-form-data-part-1/_static/image1.png)

<span data-ttu-id="5e466-138">Gdy użytkownik kliknie przycisk Prześlij, przeglądarka wyśle żądanie HTTP podobne do następujących:</span><span class="sxs-lookup"><span data-stu-id="5e466-138">When the user clicks Submit, the browser sends an HTTP request similar to the following:</span></span>

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

<span data-ttu-id="5e466-139">Należy zauważyć, że treść żądania zawiera dane formularza sformatowane jako pary nazwa/wartość.</span><span class="sxs-lookup"><span data-stu-id="5e466-139">Notice that the request body contains the form data, formatted as name/value pairs.</span></span> <span data-ttu-id="5e466-140">Interfejs API sieci Web automatycznie konwertuje pary nazwa/wartość na wystąpienie klasy `Update`.</span><span class="sxs-lookup"><span data-stu-id="5e466-140">Web API automatically converts the name/value pairs into an instance of the `Update` class.</span></span>

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a><span data-ttu-id="5e466-141">Wysyłanie danych formularza za pośrednictwem technologii AJAX</span><span class="sxs-lookup"><span data-stu-id="5e466-141">Sending Form Data via AJAX</span></span>

<span data-ttu-id="5e466-142">Gdy użytkownik przesyła formularz, przeglądarka nawiguje poza bieżącą stronę i renderuje treść komunikatu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="5e466-142">When a user submits a form, the browser navigates away from the current page and renders the body of the response message.</span></span> <span data-ttu-id="5e466-143">Jest to prawidłowe, gdy odpowiedź jest stroną HTML.</span><span class="sxs-lookup"><span data-stu-id="5e466-143">That's OK when the response is an HTML page.</span></span> <span data-ttu-id="5e466-144">W przypadku interfejsu API sieci Web treść odpowiedzi jest zwykle pusta lub zawiera dane strukturalne, takie jak JSON.</span><span class="sxs-lookup"><span data-stu-id="5e466-144">With a web API, however, the response body is usually either empty or contains structured data, such as JSON.</span></span> <span data-ttu-id="5e466-145">W takim przypadku lepiej jest wysyłać dane formularza przy użyciu żądania AJAX, aby strona mogła przetworzyć odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="5e466-145">In that case, it makes more sense to send the form data using an AJAX request, so that the page can process the response.</span></span>

<span data-ttu-id="5e466-146">Poniższy kod przedstawia sposób publikowania danych formularza przy użyciu jQuery.</span><span class="sxs-lookup"><span data-stu-id="5e466-146">The following code shows how to post form data using jQuery.</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

<span data-ttu-id="5e466-147">Funkcja **przesyłania** jQuery zastępuje akcję formularza nową funkcją.</span><span class="sxs-lookup"><span data-stu-id="5e466-147">The jQuery **submit** function replaces the form action with a new function.</span></span> <span data-ttu-id="5e466-148">Spowoduje to zastąpienie domyślnego zachowania przycisku Prześlij.</span><span class="sxs-lookup"><span data-stu-id="5e466-148">This overrides the default behavior of the Submit button.</span></span> <span data-ttu-id="5e466-149">Funkcja **serializacji** serializować dane formularza do par nazwa/wartość.</span><span class="sxs-lookup"><span data-stu-id="5e466-149">The **serialize** function serializes the form data into name/value pairs.</span></span> <span data-ttu-id="5e466-150">Aby wysłać dane formularza do serwera, wywołaj `$.post()`.</span><span class="sxs-lookup"><span data-stu-id="5e466-150">To send the form data to the server, call `$.post()`.</span></span>

<span data-ttu-id="5e466-151">Po zakończeniu żądania `.success()` lub procedura obsługi `.error()` wyświetla odpowiedni komunikat dla użytkownika.</span><span class="sxs-lookup"><span data-stu-id="5e466-151">When the request completes, the `.success()` or `.error()` handler displays an appropriate message to the user.</span></span>

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a><span data-ttu-id="5e466-152">Wysyłanie typów prostych</span><span class="sxs-lookup"><span data-stu-id="5e466-152">Sending Simple Types</span></span>

<span data-ttu-id="5e466-153">W poprzednich sekcjach wysłaliśmy typ złożony, który jest deserializowany przez internetowy interfejs API do wystąpienia klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="5e466-153">In the previous sections, we sent a complex type, which Web API deserialized to an instance of a model class.</span></span> <span data-ttu-id="5e466-154">Możesz również wysyłać typy proste, takie jak ciąg.</span><span class="sxs-lookup"><span data-stu-id="5e466-154">You can also send simple types, such as a string.</span></span>

> [!NOTE]
> <span data-ttu-id="5e466-155">Przed wysłaniem typu prostego Rozważ zamiast tego zapakowanie wartości w typie złożonym.</span><span class="sxs-lookup"><span data-stu-id="5e466-155">Before sending a simple type, consider wrapping the value in a complex type instead.</span></span> <span data-ttu-id="5e466-156">Dzięki temu można sprawdzić poprawność modelu po stronie serwera i ułatwić rozbudowanie modelu w razie potrzeby.</span><span class="sxs-lookup"><span data-stu-id="5e466-156">This gives you the benefits of model validation on the server side, and makes it easier to extend your model if needed.</span></span>

<span data-ttu-id="5e466-157">Podstawowe kroki umożliwiające wysłanie typu prostego są takie same, ale istnieją dwie delikatne różnice.</span><span class="sxs-lookup"><span data-stu-id="5e466-157">The basic steps to send a simple type are the same, but there are two subtle differences.</span></span> <span data-ttu-id="5e466-158">Najpierw w kontrolerze należy dekorować nazwę parametru z atrybutem **FromBody** .</span><span class="sxs-lookup"><span data-stu-id="5e466-158">First, in the controller, you must decorate the parameter name with the **FromBody** attribute.</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

<span data-ttu-id="5e466-159">Domyślnie interfejs API sieci Web próbuje uzyskać proste typy z identyfikatora URI żądania.</span><span class="sxs-lookup"><span data-stu-id="5e466-159">By default, Web API tries to get simple types from the request URI.</span></span> <span data-ttu-id="5e466-160">Atrybut **FromBody** informuje interfejs API sieci Web o odczytaniu wartości z treści żądania.</span><span class="sxs-lookup"><span data-stu-id="5e466-160">The **FromBody** attribute tells Web API to read the value from the request body.</span></span>

> [!NOTE]
> <span data-ttu-id="5e466-161">Interfejs API sieci Web odczytuje treść odpowiedzi najwyżej raz, więc tylko jeden parametr akcji może pochodzić z treści żądania.</span><span class="sxs-lookup"><span data-stu-id="5e466-161">Web API reads the response body at most once, so only one parameter of an action can come from the request body.</span></span> <span data-ttu-id="5e466-162">Jeśli musisz uzyskać wiele wartości z treści żądania, zdefiniuj typ złożony.</span><span class="sxs-lookup"><span data-stu-id="5e466-162">If you need to get multiple values from the request body, define a complex type.</span></span>

<span data-ttu-id="5e466-163">Po drugie klient musi wysłać wartość w następującym formacie:</span><span class="sxs-lookup"><span data-stu-id="5e466-163">Second, the client needs to send the value with the following format:</span></span>

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

<span data-ttu-id="5e466-164">W odniesieniu do typu prostego część nazwy pary nazwa/wartość musi być pusta.</span><span class="sxs-lookup"><span data-stu-id="5e466-164">Specifically, the name portion of the name/value pair must be empty for a simple type.</span></span> <span data-ttu-id="5e466-165">Nie wszystkie przeglądarki obsługują tę procedurę dla formularzy HTML, ale ten format można utworzyć w skrypcie w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="5e466-165">Not all browsers support this for HTML forms, but you create this format in script as follows:</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

<span data-ttu-id="5e466-166">Oto przykładowa postać:</span><span class="sxs-lookup"><span data-stu-id="5e466-166">Here is an example form:</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

<span data-ttu-id="5e466-167">A Oto skrypt do przesyłania wartości formularza.</span><span class="sxs-lookup"><span data-stu-id="5e466-167">And here is the script to submit the form value.</span></span> <span data-ttu-id="5e466-168">Jedyną różnicą od poprzedniego skryptu jest argument przekazaną do funkcji **post** .</span><span class="sxs-lookup"><span data-stu-id="5e466-168">The only difference from the previous script is the argument passed into the **post** function.</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

<span data-ttu-id="5e466-169">Możesz użyć tego samego podejścia, aby wysłać tablicę typów prostych:</span><span class="sxs-lookup"><span data-stu-id="5e466-169">You can use the same approach to send an array of simple types:</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a><span data-ttu-id="5e466-170">Dodatkowe materiały</span><span class="sxs-lookup"><span data-stu-id="5e466-170">Additional Resources</span></span>

[<span data-ttu-id="5e466-171">Część 2: przekazywanie plików i wieloczęściowe MIME</span><span class="sxs-lookup"><span data-stu-id="5e466-171">Part 2: File Upload and Multipart MIME</span></span>](sending-html-form-data-part-2.md)
