---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: 'Wysyłanie danych formularza HTML we wzorcu ASP.NET Web API: Dane zakodowane — ASP.NET 4.x'
author: MikeWasson
description: W tym artykule pokazano, jak publikować dane zakodowane do kontrolera interfejsu API sieci Web programu ASP.NET 4.x
ms.author: riande
ms.date: 06/15/2012
ms.custom: seoapril2019
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 7243069dbd8051b1374ed6e0112c273b8fe26f61
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65115470"
---
# <a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a><span data-ttu-id="72e6d-103">Wysyłanie danych formularza HTML we wzorcu ASP.NET Web API: dane w formacie form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="72e6d-103">Sending HTML Form Data in ASP.NET Web API: Form-urlencoded Data</span></span>

<span data-ttu-id="72e6d-104">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="72e6d-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

## <a name="part-1-form-urlencoded-data"></a><span data-ttu-id="72e6d-105">Część 1. dane w formacie form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="72e6d-105">Part 1: Form-urlencoded Data</span></span>

<span data-ttu-id="72e6d-106">W tym artykule pokazano, jak publikować dane zakodowane w kontrolerze interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="72e6d-106">This article shows how to post form-urlencoded data to a Web API controller.</span></span>

- [<span data-ttu-id="72e6d-107">Przegląd formularzy HTML</span><span class="sxs-lookup"><span data-stu-id="72e6d-107">Overview of HTML Forms</span></span>](#overview_of_html_forms)
- [<span data-ttu-id="72e6d-108">Wysyłanie typy złożone</span><span class="sxs-lookup"><span data-stu-id="72e6d-108">Sending Complex Types</span></span>](#sending_complex_types)
- [<span data-ttu-id="72e6d-109">Wysyłanie danych formularza za pomocą technologii AJAX</span><span class="sxs-lookup"><span data-stu-id="72e6d-109">Sending Form Data via AJAX</span></span>](#sending_form_data_via_ajax)
- [<span data-ttu-id="72e6d-110">Wysyłanie typy proste</span><span class="sxs-lookup"><span data-stu-id="72e6d-110">Sending Simple Types</span></span>](#sending_simple_types)

> [!NOTE]
> <span data-ttu-id="72e6d-111">[Pobieranie ukończone projektu](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).</span><span class="sxs-lookup"><span data-stu-id="72e6d-111">[Download the completed project](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).</span></span>

<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a><span data-ttu-id="72e6d-112">Przegląd formularzy HTML</span><span class="sxs-lookup"><span data-stu-id="72e6d-112">Overview of HTML Forms</span></span>

<span data-ttu-id="72e6d-113">Użyj formularzy HTML Uzyskaj lub POST do wysyłania danych do serwera.</span><span class="sxs-lookup"><span data-stu-id="72e6d-113">HTML forms use either GET or POST to send data to the server.</span></span> <span data-ttu-id="72e6d-114">**Metoda** atrybutu **formularza** elementu udostępnia metodę HTTP:</span><span class="sxs-lookup"><span data-stu-id="72e6d-114">The **method** attribute of the **form** element gives the HTTP method:</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

<span data-ttu-id="72e6d-115">Domyślną metodą jest metoda GET.</span><span class="sxs-lookup"><span data-stu-id="72e6d-115">The default method is GET.</span></span> <span data-ttu-id="72e6d-116">Formularz używa otrzymują, formularza, do którego dane są kodowane w identyfikatorze URI jako ciąg zapytania.</span><span class="sxs-lookup"><span data-stu-id="72e6d-116">If the form uses GET, the form data is encoded in the URI as a query string.</span></span> <span data-ttu-id="72e6d-117">Jeśli formularz korzysta z wpisu, dane formularza jest umieszczany w treści żądania.</span><span class="sxs-lookup"><span data-stu-id="72e6d-117">If the form uses POST, the form data is placed in the request body.</span></span> <span data-ttu-id="72e6d-118">W przypadku danych zaksięgowana **typ kodowania** atrybut określa format treści żądania:</span><span class="sxs-lookup"><span data-stu-id="72e6d-118">For POSTed data, the **enctype** attribute specifies the format of the request body:</span></span>

| <span data-ttu-id="72e6d-119">Typ kodowania</span><span class="sxs-lookup"><span data-stu-id="72e6d-119">enctype</span></span> | <span data-ttu-id="72e6d-120">Opis</span><span class="sxs-lookup"><span data-stu-id="72e6d-120">Description</span></span> |
| --- | --- |
| <span data-ttu-id="72e6d-121">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="72e6d-121">application/x-www-form-urlencoded</span></span> | <span data-ttu-id="72e6d-122">Dane formularza jest zakodowane jako pary nazwa/wartość, podobnie jak ciąg zapytania URI.</span><span class="sxs-lookup"><span data-stu-id="72e6d-122">Form data is encoded as name/value pairs, similar to a URI query string.</span></span> <span data-ttu-id="72e6d-123">Jest to domyślny format wpisu.</span><span class="sxs-lookup"><span data-stu-id="72e6d-123">This is the default format for POST.</span></span> |
| <span data-ttu-id="72e6d-124">multipart/formularza data</span><span class="sxs-lookup"><span data-stu-id="72e6d-124">multipart/form-data</span></span> | <span data-ttu-id="72e6d-125">Dane formularza jest zakodowane jako wieloczęściowej wiadomości MIME.</span><span class="sxs-lookup"><span data-stu-id="72e6d-125">Form data is encoded as a multipart MIME message.</span></span> <span data-ttu-id="72e6d-126">Użyj tego formatu, jeśli plik jest przekazywany do serwera.</span><span class="sxs-lookup"><span data-stu-id="72e6d-126">Use this format if you are uploading a file to the server.</span></span> |

<span data-ttu-id="72e6d-127">Część 1 w tym artykule patrzy na format x--www-form-urlencoded.</span><span class="sxs-lookup"><span data-stu-id="72e6d-127">Part 1 of this article looks at x-www-form-urlencoded format.</span></span> <span data-ttu-id="72e6d-128">[Część 2](sending-html-form-data-part-2.md) opisuje wieloczęściowej wiadomości MIME.</span><span class="sxs-lookup"><span data-stu-id="72e6d-128">[Part 2](sending-html-form-data-part-2.md) describes multipart MIME.</span></span>

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a><span data-ttu-id="72e6d-129">Wysyłanie typy złożone</span><span class="sxs-lookup"><span data-stu-id="72e6d-129">Sending Complex Types</span></span>

<span data-ttu-id="72e6d-130">Zazwyczaj będzie wysyłać typ złożony, zawierający wartości z kilku kontrolek formularza.</span><span class="sxs-lookup"><span data-stu-id="72e6d-130">Typically, you will send a complex type, composed of values taken from several form controls.</span></span> <span data-ttu-id="72e6d-131">Należy wziąć pod uwagę następujące modelu, który reprezentuje aktualizację stanu:</span><span class="sxs-lookup"><span data-stu-id="72e6d-131">Consider the following model that represents a status update:</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

<span data-ttu-id="72e6d-132">Poniżej przedstawiono kontroler internetowego interfejsu API, który akceptuje `Update` obiektu za pomocą wpisu.</span><span class="sxs-lookup"><span data-stu-id="72e6d-132">Here is a Web API controller that accepts an `Update` object via POST.</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> <span data-ttu-id="72e6d-133">Korzysta z tego kontrolera [routing oparty na akcję](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), więc szablon trasy jest &quot;interfejsu api / {controller} / {action} / {id}&quot;.</span><span class="sxs-lookup"><span data-stu-id="72e6d-133">This controller uses [action-based routing](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), so the route template is &quot;api/{controller}/{action}/{id}&quot;.</span></span> <span data-ttu-id="72e6d-134">Klient będzie wysyłania danych do &quot;/api/updates/complex&quot;.</span><span class="sxs-lookup"><span data-stu-id="72e6d-134">The client will post the data to &quot;/api/updates/complex&quot;.</span></span>

<span data-ttu-id="72e6d-135">Teraz napiszmy formularza HTML służący do przesyłania aktualizacji stanu.</span><span class="sxs-lookup"><span data-stu-id="72e6d-135">Now let's write an HTML form for users to submit a status update.</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

<span data-ttu-id="72e6d-136">Należy zauważyć, że **akcji** w formie znajduje się atrybut identyfikatora URI o naszej akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="72e6d-136">Notice that the **action** attribute on the form is the URI of our controller action.</span></span> <span data-ttu-id="72e6d-137">Poniżej przedstawiono formularz z niektórych wartościami podanymi w:</span><span class="sxs-lookup"><span data-stu-id="72e6d-137">Here is the form with some values entered in:</span></span>

![](sending-html-form-data-part-1/_static/image1.png)

<span data-ttu-id="72e6d-138">Gdy użytkownik kliknie przycisk Prześlij, przeglądarce wysyła żądanie HTTP podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="72e6d-138">When the user clicks Submit, the browser sends an HTTP request similar to the following:</span></span>

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

<span data-ttu-id="72e6d-139">Należy zauważyć, że treść żądania zawiera dane formularza sformatowane jako pary nazwa/wartość.</span><span class="sxs-lookup"><span data-stu-id="72e6d-139">Notice that the request body contains the form data, formatted as name/value pairs.</span></span> <span data-ttu-id="72e6d-140">Interfejs API sieci Web automatycznie konwertuje pary nazwa/wartość na wystąpienie `Update` klasy.</span><span class="sxs-lookup"><span data-stu-id="72e6d-140">Web API automatically converts the name/value pairs into an instance of the `Update` class.</span></span>

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a><span data-ttu-id="72e6d-141">Wysyłanie danych formularza za pomocą technologii AJAX</span><span class="sxs-lookup"><span data-stu-id="72e6d-141">Sending Form Data via AJAX</span></span>

<span data-ttu-id="72e6d-142">Gdy użytkownik przesyła formularz, przeglądarka przechodzi od bieżącej strony i renderuje treści komunikatu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="72e6d-142">When a user submits a form, the browser navigates away from the current page and renders the body of the response message.</span></span> <span data-ttu-id="72e6d-143">To normalne, gdy odpowiedź jest stroną HTML.</span><span class="sxs-lookup"><span data-stu-id="72e6d-143">That's OK when the response is an HTML page.</span></span> <span data-ttu-id="72e6d-144">Za pomocą internetowego interfejsu API, jednak treść odpowiedzi jest zwykle albo pusta lub zawiera dane strukturalnych, takich jak JSON.</span><span class="sxs-lookup"><span data-stu-id="72e6d-144">With a web API, however, the response body is usually either empty or contains structured data, such as JSON.</span></span> <span data-ttu-id="72e6d-145">W takim przypadku warto więcej wysyłania żądania danych formularza za pomocą interfejsu AJAX, dzięki czemu strony może przetwarzać odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="72e6d-145">In that case, it makes more sense to send the form data using an AJAX request, so that the page can process the response.</span></span>

<span data-ttu-id="72e6d-146">Poniższy kod pokazuje, jak publikować dane formularza przy użyciu jQuery.</span><span class="sxs-lookup"><span data-stu-id="72e6d-146">The following code shows how to post form data using jQuery.</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

<span data-ttu-id="72e6d-147">JQuery **przesłać** funkcja zastępuje akcji formularza za pomocą nowych funkcji.</span><span class="sxs-lookup"><span data-stu-id="72e6d-147">The jQuery **submit** function replaces the form action with a new function.</span></span> <span data-ttu-id="72e6d-148">Zastępuje to domyślne zachowanie przycisk Prześlij.</span><span class="sxs-lookup"><span data-stu-id="72e6d-148">This overrides the default behavior of the Submit button.</span></span> <span data-ttu-id="72e6d-149">**Serializacji** funkcja serializuje dane formularza do pary nazwa/wartość.</span><span class="sxs-lookup"><span data-stu-id="72e6d-149">The **serialize** function serializes the form data into name/value pairs.</span></span> <span data-ttu-id="72e6d-150">Aby wysłać dane formularza do serwera, należy wywołać `$.post()`.</span><span class="sxs-lookup"><span data-stu-id="72e6d-150">To send the form data to the server, call `$.post()`.</span></span>

<span data-ttu-id="72e6d-151">Po ukończeniu żądania `.success()` lub `.error()` obsługi wyświetla odpowiedni komunikat dla użytkownika.</span><span class="sxs-lookup"><span data-stu-id="72e6d-151">When the request completes, the `.success()` or `.error()` handler displays an appropriate message to the user.</span></span>

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a><span data-ttu-id="72e6d-152">Wysyłanie typy proste</span><span class="sxs-lookup"><span data-stu-id="72e6d-152">Sending Simple Types</span></span>

<span data-ttu-id="72e6d-153">W poprzednich sekcjach wysłaliśmy typ złożony, który internetowego interfejsu API została zdeserializowana do wystąpienia klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="72e6d-153">In the previous sections, we sent a complex type, which Web API deserialized to an instance of a model class.</span></span> <span data-ttu-id="72e6d-154">Można również wysyłać typów prostych, takie jak ciąg.</span><span class="sxs-lookup"><span data-stu-id="72e6d-154">You can also send simple types, such as a string.</span></span>

> [!NOTE]
> <span data-ttu-id="72e6d-155">Przed wysłaniem typu prostego, należy wziąć pod uwagę zamiast zawijania wartość w typie złożonym.</span><span class="sxs-lookup"><span data-stu-id="72e6d-155">Before sending a simple type, consider wrapping the value in a complex type instead.</span></span> <span data-ttu-id="72e6d-156">Zapewnia korzyści wynikające z weryfikacją modelu po stronie serwera i ułatwia Rozszerzanie modelu, jeśli to konieczne.</span><span class="sxs-lookup"><span data-stu-id="72e6d-156">This gives you the benefits of model validation on the server side, and makes it easier to extend your model if needed.</span></span>

<span data-ttu-id="72e6d-157">Podstawowe kroki, aby wysłać typu prostego są takie same, ale istnieją dwa drobne różnice.</span><span class="sxs-lookup"><span data-stu-id="72e6d-157">The basic steps to send a simple type are the same, but there are two subtle differences.</span></span> <span data-ttu-id="72e6d-158">Najpierw na kontrolerze musi dekorowania nazwy parametru z **FromBody** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="72e6d-158">First, in the controller, you must decorate the parameter name with the **FromBody** attribute.</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

<span data-ttu-id="72e6d-159">Domyślnie interfejs API sieci Web próbuje uzyskać proste typy z identyfikatora URI żądania.</span><span class="sxs-lookup"><span data-stu-id="72e6d-159">By default, Web API tries to get simple types from the request URI.</span></span> <span data-ttu-id="72e6d-160">**FromBody** atrybut informuje interfejsu API sieci Web ma zostać odczytana wartość z treści żądania.</span><span class="sxs-lookup"><span data-stu-id="72e6d-160">The **FromBody** attribute tells Web API to read the value from the request body.</span></span>

> [!NOTE]
> <span data-ttu-id="72e6d-161">Interfejs API sieci Web odczytuje treść odpowiedzi co najwyżej jeden raz, dlatego tylko jeden parametr akcji mogą pochodzić z treści żądania.</span><span class="sxs-lookup"><span data-stu-id="72e6d-161">Web API reads the response body at most once, so only one parameter of an action can come from the request body.</span></span> <span data-ttu-id="72e6d-162">Jeśli musisz pobrać wiele wartości z treści żądania, należy zdefiniować typ złożony.</span><span class="sxs-lookup"><span data-stu-id="72e6d-162">If you need to get multiple values from the request body, define a complex type.</span></span>

<span data-ttu-id="72e6d-163">Po drugie klient musi wysyłać wartość w następującym formacie:</span><span class="sxs-lookup"><span data-stu-id="72e6d-163">Second, the client needs to send the value with the following format:</span></span>

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

<span data-ttu-id="72e6d-164">W szczególności część nazwy pary nazwa/wartość musi być pusty dla typu prostego.</span><span class="sxs-lookup"><span data-stu-id="72e6d-164">Specifically, the name portion of the name/value pair must be empty for a simple type.</span></span> <span data-ttu-id="72e6d-165">Nie wszystkie przeglądarki są obsługiwane przez to formularzy HTML, ale utworzysz ten format w skrypcie w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="72e6d-165">Not all browsers support this for HTML forms, but you create this format in script as follows:</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

<span data-ttu-id="72e6d-166">Oto przykład formularza:</span><span class="sxs-lookup"><span data-stu-id="72e6d-166">Here is an example form:</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

<span data-ttu-id="72e6d-167">A Oto skrypt, aby przesłać wartości formularza.</span><span class="sxs-lookup"><span data-stu-id="72e6d-167">And here is the script to submit the form value.</span></span> <span data-ttu-id="72e6d-168">Jedyną różnicą między poprzednim skrypcie jest argument przekazany do **wpis** funkcji.</span><span class="sxs-lookup"><span data-stu-id="72e6d-168">The only difference from the previous script is the argument passed into the **post** function.</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

<span data-ttu-id="72e6d-169">To samo podejście umożliwia wysyłanie tablicę typów prostych:</span><span class="sxs-lookup"><span data-stu-id="72e6d-169">You can use the same approach to send an array of simple types:</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a><span data-ttu-id="72e6d-170">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="72e6d-170">Additional Resources</span></span>

[<span data-ttu-id="72e6d-171">Część 2. Przekazywanie pliku i wieloczęściowej wiadomości MIME</span><span class="sxs-lookup"><span data-stu-id="72e6d-171">Part 2: File Upload and Multipart MIME</span></span>](sending-html-form-data-part-2.md)
