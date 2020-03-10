---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: Wywoływanie internetowego interfejsu API z klienta platformy .NETC#() — ASP.NET 4. x
author: MikeWasson
description: W tym samouczku pokazano, jak wywołać interfejs API sieci Web z aplikacji .NET 4. x.
ms.author: riande
ms.date: 11/24/2017
ms.custom: seoapril2019
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: ab3ba71839123e848dffaa59871f9dac8c1a88d0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622620"
---
# <a name="call-a-web-api-from-a-net-client-c"></a><span data-ttu-id="656ca-103">Wywoływanie internetowego interfejsu API z klienta platformy .NETC#()</span><span class="sxs-lookup"><span data-stu-id="656ca-103">Call a Web API From a .NET Client (C#)</span></span>

<span data-ttu-id="656ca-104">według [Jan Wasson](https://github.com/MikeWasson) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="656ca-104">by [Mike Wasson](https://github.com/MikeWasson) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="656ca-105">[Pobierz ukończony projekt](https://github.com/dotnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample).</span><span class="sxs-lookup"><span data-stu-id="656ca-105">[Download Completed Project](https://github.com/dotnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample).</span></span> <span data-ttu-id="656ca-106">[Instrukcje pobierania](/aspnet/core/tutorials/#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="656ca-106">[Download instructions](/aspnet/core/tutorials/#how-to-download-a-sample).</span></span> 

<span data-ttu-id="656ca-107">W tym samouczku pokazano, jak wywołać interfejs API sieci Web z aplikacji .NET przy użyciu [systemu .NET. http. HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)</span><span class="sxs-lookup"><span data-stu-id="656ca-107">This tutorial shows how to call a web API from a .NET application, using [System.Net.Http.HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)</span></span>

<span data-ttu-id="656ca-108">W tym samouczku aplikacja kliencka jest zapisywana, która wykorzystuje następujący internetowy interfejs API:</span><span class="sxs-lookup"><span data-stu-id="656ca-108">In this tutorial, a client app is written that consumes the following web API:</span></span>

| <span data-ttu-id="656ca-109">Akcja</span><span class="sxs-lookup"><span data-stu-id="656ca-109">Action</span></span> | <span data-ttu-id="656ca-110">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="656ca-110">HTTP method</span></span> | <span data-ttu-id="656ca-111">Względny identyfikator URI</span><span class="sxs-lookup"><span data-stu-id="656ca-111">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="656ca-112">Pobierz produkt według identyfikatora</span><span class="sxs-lookup"><span data-stu-id="656ca-112">Get a product by ID</span></span> | <span data-ttu-id="656ca-113">GET</span><span class="sxs-lookup"><span data-stu-id="656ca-113">GET</span></span> | <span data-ttu-id="656ca-114">*Identyfikator* /API/Products/</span><span class="sxs-lookup"><span data-stu-id="656ca-114">/api/products/*id*</span></span> |
| <span data-ttu-id="656ca-115">Utwórz nowy produkt</span><span class="sxs-lookup"><span data-stu-id="656ca-115">Create a new product</span></span> | <span data-ttu-id="656ca-116">POST</span><span class="sxs-lookup"><span data-stu-id="656ca-116">POST</span></span> | <span data-ttu-id="656ca-117">/api/products</span><span class="sxs-lookup"><span data-stu-id="656ca-117">/api/products</span></span> |
| <span data-ttu-id="656ca-118">Aktualizowanie produktu</span><span class="sxs-lookup"><span data-stu-id="656ca-118">Update a product</span></span> | <span data-ttu-id="656ca-119">PUT</span><span class="sxs-lookup"><span data-stu-id="656ca-119">PUT</span></span> | <span data-ttu-id="656ca-120">*Identyfikator* /API/Products/</span><span class="sxs-lookup"><span data-stu-id="656ca-120">/api/products/*id*</span></span> |
| <span data-ttu-id="656ca-121">Usuwanie produktu</span><span class="sxs-lookup"><span data-stu-id="656ca-121">Delete a product</span></span> | <span data-ttu-id="656ca-122">DELETE</span><span class="sxs-lookup"><span data-stu-id="656ca-122">DELETE</span></span> | <span data-ttu-id="656ca-123">*Identyfikator* /API/Products/</span><span class="sxs-lookup"><span data-stu-id="656ca-123">/api/products/*id*</span></span> |

<span data-ttu-id="656ca-124">Aby dowiedzieć się, jak zaimplementować ten interfejs API za pomocą interfejsu API sieci Web ASP.NET, zobacz [Tworzenie internetowego interfejsu API obsługującego operacje CRUD](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span><span class="sxs-lookup"><span data-stu-id="656ca-124">To learn how to implement this API with ASP.NET Web API, see [Creating a Web API that Supports CRUD Operations](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span></span>

<span data-ttu-id="656ca-125">Dla uproszczenia aplikacja kliencka w tym samouczku jest aplikacją konsolową dla systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="656ca-125">For simplicity, the client application in this tutorial is a Windows console application.</span></span> <span data-ttu-id="656ca-126">**HttpClient** jest również obsługiwana dla Windows Phone i aplikacji ze sklepu Windows.</span><span class="sxs-lookup"><span data-stu-id="656ca-126">**HttpClient** is also supported for Windows Phone and Windows Store apps.</span></span> <span data-ttu-id="656ca-127">Aby uzyskać więcej informacji, zobacz [pisanie kodu klienta interfejsu API sieci Web dla wielu platform przy użyciu bibliotek przenośnych](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span><span class="sxs-lookup"><span data-stu-id="656ca-127">For more information, see [Writing Web API Client Code for Multiple Platforms Using Portable Libraries](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span></span>

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a><span data-ttu-id="656ca-128">Tworzenie aplikacji konsolowej</span><span class="sxs-lookup"><span data-stu-id="656ca-128">Create the Console Application</span></span>

<span data-ttu-id="656ca-129">W programie Visual Studio Utwórz nową aplikację konsolową systemu Windows o nazwie **HttpClientSample** i wklej ją w następującym kodzie:</span><span class="sxs-lookup"><span data-stu-id="656ca-129">In Visual Studio, create a new Windows console app named **HttpClientSample** and paste in the following code:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

<span data-ttu-id="656ca-130">Poprzedni kod jest kompletną aplikacją kliencką.</span><span class="sxs-lookup"><span data-stu-id="656ca-130">The preceding code is the complete client app.</span></span>

<span data-ttu-id="656ca-131">`RunAsync` działa i blokuje do momentu ukończenia.</span><span class="sxs-lookup"><span data-stu-id="656ca-131">`RunAsync` runs and blocks until it completes.</span></span> <span data-ttu-id="656ca-132">Większość metod **HttpClient** jest asynchroniczna, ponieważ wykonują we/wy sieci.</span><span class="sxs-lookup"><span data-stu-id="656ca-132">Most **HttpClient** methods are async, because they perform network I/O.</span></span> <span data-ttu-id="656ca-133">Wszystkie zadania asynchroniczne są wykonywane w `RunAsync`.</span><span class="sxs-lookup"><span data-stu-id="656ca-133">All of the async tasks are done inside `RunAsync`.</span></span> <span data-ttu-id="656ca-134">Zwykle aplikacja nie blokuje wątku głównego, ale ta aplikacja nie zezwala na interakcję.</span><span class="sxs-lookup"><span data-stu-id="656ca-134">Normally an app doesn't block the main thread, but this app doesn't allow any interaction.</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a><span data-ttu-id="656ca-135">Instalowanie bibliotek klienckich interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="656ca-135">Install the Web API Client Libraries</span></span>

<span data-ttu-id="656ca-136">Zainstaluj pakiet biblioteki klienta internetowego interfejsu API za pomocą Menedżera pakietów NuGet.</span><span class="sxs-lookup"><span data-stu-id="656ca-136">Use NuGet Package Manager to install the Web API Client Libraries package.</span></span>

<span data-ttu-id="656ca-137">W menu **Narzędzia** wybierz kolejno pozycje **menedżer pakietów NuGet** > **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="656ca-137">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="656ca-138">W konsoli Menedżera pakietów (PMC) wpisz następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="656ca-138">In the Package Manager Console (PMC), type the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.Client`

<span data-ttu-id="656ca-139">Poprzednie polecenie dodaje następujące pakiety NuGet do projektu:</span><span class="sxs-lookup"><span data-stu-id="656ca-139">The preceding command adds the following NuGet packages to the project:</span></span>

* <span data-ttu-id="656ca-140">Microsoft.AspNet.WebApi.Client</span><span class="sxs-lookup"><span data-stu-id="656ca-140">Microsoft.AspNet.WebApi.Client</span></span>
* <span data-ttu-id="656ca-141">Newtonsoft.Json</span><span class="sxs-lookup"><span data-stu-id="656ca-141">Newtonsoft.Json</span></span>

<span data-ttu-id="656ca-142">Netwonsoft. JSON (znany również jako Json.NET) to popularne środowisko JSON o wysokiej wydajności dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="656ca-142">Netwonsoft.Json (also known as Json.NET) is a popular high-performance JSON framework for .NET.</span></span>

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a><span data-ttu-id="656ca-143">Dodaj klasę modelu</span><span class="sxs-lookup"><span data-stu-id="656ca-143">Add a Model Class</span></span>

<span data-ttu-id="656ca-144">Zapoznaj się z klasą `Product`:</span><span class="sxs-lookup"><span data-stu-id="656ca-144">Examine the `Product` class:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

<span data-ttu-id="656ca-145">Ta klasa jest zgodna z modelem danych używanym przez internetowy interfejs API.</span><span class="sxs-lookup"><span data-stu-id="656ca-145">This class matches the data model used by the web API.</span></span> <span data-ttu-id="656ca-146">Aplikacja może używać **HttpClient** , aby odczytywać wystąpienie `Product` z odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="656ca-146">An app can use **HttpClient** to read a `Product` instance from an HTTP response.</span></span> <span data-ttu-id="656ca-147">Aplikacja nie musi pisać kodu deserializacji.</span><span class="sxs-lookup"><span data-stu-id="656ca-147">The app doesn't have to write any deserialization code.</span></span>

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a><span data-ttu-id="656ca-148">Utwórz i zainicjuj HttpClient</span><span class="sxs-lookup"><span data-stu-id="656ca-148">Create and Initialize HttpClient</span></span>

<span data-ttu-id="656ca-149">Przejrzyj statyczną Właściwość **HttpClient** :</span><span class="sxs-lookup"><span data-stu-id="656ca-149">Examine the static **HttpClient** property:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

<span data-ttu-id="656ca-150">**HttpClient** jest przeznaczony do tworzenia wystąpień i ponownie używany przez cały czas życia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="656ca-150">**HttpClient** is intended to be instantiated once and reused throughout the life of an application.</span></span> <span data-ttu-id="656ca-151">Następujące warunki mogą spowodować błędy **gniazdaexception** :</span><span class="sxs-lookup"><span data-stu-id="656ca-151">The following conditions can result in **SocketException** errors:</span></span>

* <span data-ttu-id="656ca-152">Tworzenie nowego wystąpienia **HttpClient** na żądanie.</span><span class="sxs-lookup"><span data-stu-id="656ca-152">Creating a new **HttpClient** instance per request.</span></span>
* <span data-ttu-id="656ca-153">Serwer z dużym obciążeniem.</span><span class="sxs-lookup"><span data-stu-id="656ca-153">Server under heavy load.</span></span>

<span data-ttu-id="656ca-154">Utworzenie nowego wystąpienia **HttpClient** na żądanie może obsłużyć dostępne gniazda.</span><span class="sxs-lookup"><span data-stu-id="656ca-154">Creating a new **HttpClient** instance per request can exhaust the available sockets.</span></span>

<span data-ttu-id="656ca-155">Poniższy kod Inicjuje wystąpienie **HttpClient** :</span><span class="sxs-lookup"><span data-stu-id="656ca-155">The following code initializes the **HttpClient** instance:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

<span data-ttu-id="656ca-156">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="656ca-156">The preceding code:</span></span>

* <span data-ttu-id="656ca-157">Ustawia podstawowy identyfikator URI dla żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="656ca-157">Sets the base URI for HTTP requests.</span></span> <span data-ttu-id="656ca-158">Zmień numer portu na port używany w aplikacji serwera.</span><span class="sxs-lookup"><span data-stu-id="656ca-158">Change the port number to the port used in the server app.</span></span> <span data-ttu-id="656ca-159">Aplikacja nie będzie działała, chyba że zostanie użyty port dla aplikacji serwera.</span><span class="sxs-lookup"><span data-stu-id="656ca-159">The app won't work unless port for the server app is used.</span></span>
* <span data-ttu-id="656ca-160">Ustawia nagłówek Accept na wartość "Application/JSON".</span><span class="sxs-lookup"><span data-stu-id="656ca-160">Sets the Accept header to "application/json".</span></span> <span data-ttu-id="656ca-161">Ustawienie tego nagłówka nakazuje serwerowi wysyłanie danych w formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="656ca-161">Setting this header tells the server to send data in JSON format.</span></span>

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a><span data-ttu-id="656ca-162">Wysyłanie żądania GET w celu pobrania zasobu</span><span class="sxs-lookup"><span data-stu-id="656ca-162">Send a GET request to retrieve a resource</span></span>

<span data-ttu-id="656ca-163">Poniższy kod wysyła żądanie GET dla produktu:</span><span class="sxs-lookup"><span data-stu-id="656ca-163">The following code sends a GET request for a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

<span data-ttu-id="656ca-164">Metoda **GetAsync** wysyła żądanie HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="656ca-164">The **GetAsync** method sends the HTTP GET request.</span></span> <span data-ttu-id="656ca-165">Gdy metoda zostanie ukończona, zwraca **HttpResponseMessage** , która zawiera odpowiedź HTTP.</span><span class="sxs-lookup"><span data-stu-id="656ca-165">When the method completes, it returns an **HttpResponseMessage** that contains the HTTP response.</span></span> <span data-ttu-id="656ca-166">Jeśli kod stanu w odpowiedzi to kod sukcesu, treść odpowiedzi zawiera reprezentację w formacie JSON produktu.</span><span class="sxs-lookup"><span data-stu-id="656ca-166">If the status code in the response is a success code, the response body contains the JSON representation of a product.</span></span> <span data-ttu-id="656ca-167">Wywołaj **ReadAsAsync** , aby ZDESERIALIZOWAĆ ładunek JSON do wystąpienia `Product`.</span><span class="sxs-lookup"><span data-stu-id="656ca-167">Call **ReadAsAsync** to deserialize the JSON payload to a `Product` instance.</span></span> <span data-ttu-id="656ca-168">Metoda **ReadAsAsync** jest asynchroniczna, ponieważ treść odpowiedzi może być arbitralnie duża.</span><span class="sxs-lookup"><span data-stu-id="656ca-168">The **ReadAsAsync** method is asynchronous because the response body can be arbitrarily large.</span></span>

<span data-ttu-id="656ca-169">**HttpClient** nie zgłasza wyjątku, gdy odpowiedź HTTP zawiera kod błędu.</span><span class="sxs-lookup"><span data-stu-id="656ca-169">**HttpClient** does not throw an exception when the HTTP response contains an error code.</span></span> <span data-ttu-id="656ca-170">Zamiast tego Właściwość **IsSuccessStatusCode** ma **wartość false** , jeśli stan jest kodem błędu.</span><span class="sxs-lookup"><span data-stu-id="656ca-170">Instead, the **IsSuccessStatusCode** property is **false** if the status is an error code.</span></span> <span data-ttu-id="656ca-171">Jeśli wolisz traktować kody błędów HTTP jako wyjątki, wywołaj [HttpResponseMessage. EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) w obiekcie Response.</span><span class="sxs-lookup"><span data-stu-id="656ca-171">If you prefer to treat HTTP error codes as exceptions, call [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) on the response object.</span></span> <span data-ttu-id="656ca-172">`EnsureSuccessStatusCode` zgłasza wyjątek, jeśli kod stanu znajduje się poza zakresem 200&ndash;299.</span><span class="sxs-lookup"><span data-stu-id="656ca-172">`EnsureSuccessStatusCode` throws an exception if the status code falls outside the range 200&ndash;299.</span></span> <span data-ttu-id="656ca-173">Należy pamiętać, że **HttpClient** może zgłosić wyjątki z innych przyczyn &mdash; na przykład, jeśli limit czasu żądania upłynął.</span><span class="sxs-lookup"><span data-stu-id="656ca-173">Note that **HttpClient** can throw exceptions for other reasons &mdash; for example, if the request times out.</span></span>

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a><span data-ttu-id="656ca-174">Elementy formatujące typu multimediów do deserializacji</span><span class="sxs-lookup"><span data-stu-id="656ca-174">Media-Type Formatters to Deserialize</span></span>

<span data-ttu-id="656ca-175">Gdy **ReadAsAsync** jest wywoływana bez parametrów, używa domyślnego zestawu elementów *formatujących multimedia* do odczytywania treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="656ca-175">When **ReadAsAsync** is called with no parameters, it uses a default set of *media formatters* to read the response body.</span></span> <span data-ttu-id="656ca-176">Domyślne elementy formatujące obsługują dane JSON, XML i zakodowane w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="656ca-176">The default formatters support JSON, XML, and Form-url-encoded data.</span></span>

<span data-ttu-id="656ca-177">Zamiast korzystać z domyślnych elementów formatujących, można dostarczyć listę elementów formatujących do metody **ReadAsAsync** .</span><span class="sxs-lookup"><span data-stu-id="656ca-177">Instead of using the default formatters, you can provide a list of formatters to the **ReadAsAsync** method.</span></span>  <span data-ttu-id="656ca-178">Korzystanie z listy elementów formatujących jest przydatne, jeśli masz niestandardowy program formatujący typ nośnika:</span><span class="sxs-lookup"><span data-stu-id="656ca-178">Using a list of formatters is useful if you have a custom media-type formatter:</span></span>

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

<span data-ttu-id="656ca-179">Aby uzyskać więcej informacji, zobacz elementy [formatujące multimedia w ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)</span><span class="sxs-lookup"><span data-stu-id="656ca-179">For more information, see [Media Formatters in ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)</span></span>

## <a name="sending-a-post-request-to-create-a-resource"></a><span data-ttu-id="656ca-180">Wysyłanie żądania POST w celu utworzenia zasobu</span><span class="sxs-lookup"><span data-stu-id="656ca-180">Sending a POST Request to Create a Resource</span></span>

<span data-ttu-id="656ca-181">Poniższy kod wysyła żądanie POST zawierające wystąpienie `Product` w formacie JSON:</span><span class="sxs-lookup"><span data-stu-id="656ca-181">The following code sends a POST request that contains a `Product` instance in JSON format:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

<span data-ttu-id="656ca-182">Metoda **PostAsJsonAsync** :</span><span class="sxs-lookup"><span data-stu-id="656ca-182">The **PostAsJsonAsync** method:</span></span>

* <span data-ttu-id="656ca-183">Serializacja obiektu do formatu JSON.</span><span class="sxs-lookup"><span data-stu-id="656ca-183">Serializes an object to JSON.</span></span>
* <span data-ttu-id="656ca-184">Wysyła ładunek JSON w żądaniu POST.</span><span class="sxs-lookup"><span data-stu-id="656ca-184">Sends the JSON payload in a POST request.</span></span>

<span data-ttu-id="656ca-185">Jeśli żądanie zakończy się pomyślnie:</span><span class="sxs-lookup"><span data-stu-id="656ca-185">If the request succeeds:</span></span>

* <span data-ttu-id="656ca-186">Powinna zwrócić odpowiedź 201 (utworzona).</span><span class="sxs-lookup"><span data-stu-id="656ca-186">It should return a 201 (Created) response.</span></span>
* <span data-ttu-id="656ca-187">Odpowiedź powinna zawierać adres URL utworzonych zasobów w nagłówku lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="656ca-187">The response should include the URL of the created resources in the Location header.</span></span>

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a><span data-ttu-id="656ca-188">Wysyłanie żądania PUT w celu zaktualizowania zasobu</span><span class="sxs-lookup"><span data-stu-id="656ca-188">Sending a PUT Request to Update a Resource</span></span>

<span data-ttu-id="656ca-189">Poniższy kod wysyła żądanie PUT w celu zaktualizowania produktu:</span><span class="sxs-lookup"><span data-stu-id="656ca-189">The following code sends a PUT request to update a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

<span data-ttu-id="656ca-190">Metoda **PutAsJsonAsync** działa podobnie jak **PostAsJsonAsync**, z tą różnicą, że wysyła żądanie Put zamiast post.</span><span class="sxs-lookup"><span data-stu-id="656ca-190">The **PutAsJsonAsync** method works like **PostAsJsonAsync**, except that it sends a PUT request instead of POST.</span></span>

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a><span data-ttu-id="656ca-191">Wysyłanie żądania usunięcia w celu usunięcia zasobu</span><span class="sxs-lookup"><span data-stu-id="656ca-191">Sending a DELETE Request to Delete a Resource</span></span>

<span data-ttu-id="656ca-192">Poniższy kod wysyła żądanie usunięcia, aby usunąć produkt:</span><span class="sxs-lookup"><span data-stu-id="656ca-192">The following code sends a DELETE request to delete a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

<span data-ttu-id="656ca-193">Podobnie jak pobieranie, żądanie usunięcia nie ma treści żądania.</span><span class="sxs-lookup"><span data-stu-id="656ca-193">Like GET, a DELETE request does not have a request body.</span></span> <span data-ttu-id="656ca-194">Nie musisz określać formatu JSON ani XML z opcją DELETE.</span><span class="sxs-lookup"><span data-stu-id="656ca-194">You don't need to specify JSON or XML format with DELETE.</span></span>

## <a name="test-the-sample"></a><span data-ttu-id="656ca-195">Testowanie przykładu</span><span class="sxs-lookup"><span data-stu-id="656ca-195">Test the sample</span></span>

<span data-ttu-id="656ca-196">Aby przetestować aplikację kliencką:</span><span class="sxs-lookup"><span data-stu-id="656ca-196">To test the client app:</span></span>

1. <span data-ttu-id="656ca-197">[Pobierz](https://github.com/dotnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) i uruchom aplikację serwera.</span><span class="sxs-lookup"><span data-stu-id="656ca-197">[Download](https://github.com/dotnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) and run the server app.</span></span> <span data-ttu-id="656ca-198">[Instrukcje pobierania](/aspnet/core/#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="656ca-198">[Download instructions](/aspnet/core/#how-to-download-a-sample).</span></span> <span data-ttu-id="656ca-199">Sprawdź, czy aplikacja serwera działa.</span><span class="sxs-lookup"><span data-stu-id="656ca-199">Verify the server app is working.</span></span> <span data-ttu-id="656ca-200">Na przykład `http://localhost:64195/api/products` powinna zwrócić listę produktów.</span><span class="sxs-lookup"><span data-stu-id="656ca-200">For example, `http://localhost:64195/api/products` should return a list of products.</span></span>
2. <span data-ttu-id="656ca-201">Ustaw podstawowy identyfikator URI dla żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="656ca-201">Set the base URI for HTTP requests.</span></span> <span data-ttu-id="656ca-202">Zmień numer portu na port używany w aplikacji serwera.</span><span class="sxs-lookup"><span data-stu-id="656ca-202">Change the port number to the port used in the server app.</span></span>
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. <span data-ttu-id="656ca-203">Uruchom aplikację kliencką.</span><span class="sxs-lookup"><span data-stu-id="656ca-203">Run the client app.</span></span> <span data-ttu-id="656ca-204">Generowane są następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="656ca-204">The following output is produced:</span></span>

   ```console
   Created at http://localhost:64195/api/products/4
   Name: Gizmo     Price: 100.0    Category: Widgets
   Updating price...
   Name: Gizmo     Price: 80.0     Category: Widgets
   Deleted (HTTP Status = 204)
   ```
