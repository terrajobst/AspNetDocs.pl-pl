---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: Wywoływanie interfejsu API sieci Web z klienta programu .NET (C#) — ASP.NET 4.x
author: MikeWasson
description: W tym samouczku pokazano, jak wywołać interfejs API sieci web z poziomu aplikacji .NET 4.x.
ms.author: riande
ms.date: 11/24/2017
ms.custom: seoapril2019
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 113600ca1e77ae9667465464da505478fc948c9b
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59421111"
---
# <a name="call-a-web-api-from-a-net-client-c"></a><span data-ttu-id="25fc8-103">Wywoływanie interfejsu API sieci Web z klienta programu .NET (C#)</span><span class="sxs-lookup"><span data-stu-id="25fc8-103">Call a Web API From a .NET Client (C#)</span></span>

<span data-ttu-id="25fc8-104">przez [Mike Wasson](https://github.com/MikeWasson) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="25fc8-104">by [Mike Wasson](https://github.com/MikeWasson) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="25fc8-105">[Pobieranie ukończone projektu](https://github.com/aspnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample).</span><span class="sxs-lookup"><span data-stu-id="25fc8-105">[Download Completed Project](https://github.com/aspnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample).</span></span> <span data-ttu-id="25fc8-106">[Instrukcje pobierania](/aspnet/core/tutorials/#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="25fc8-106">[Download instructions](/aspnet/core/tutorials/#how-to-download-a-sample).</span></span> 

<span data-ttu-id="25fc8-107">W tym samouczku przedstawiono sposób wywoływania interfejsu API sieci web z poziomu aplikacji .NET przy użyciu [System.Net.Http.HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)</span><span class="sxs-lookup"><span data-stu-id="25fc8-107">This tutorial shows how to call a web API from a .NET application, using [System.Net.Http.HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)</span></span>

<span data-ttu-id="25fc8-108">W tym samouczku aplikacja kliencka jest zapisywane, zużywa następujący internetowy interfejs API:</span><span class="sxs-lookup"><span data-stu-id="25fc8-108">In this tutorial, a client app is written that consumes the following web API:</span></span>

| <span data-ttu-id="25fc8-109">Akcja</span><span class="sxs-lookup"><span data-stu-id="25fc8-109">Action</span></span> | <span data-ttu-id="25fc8-110">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="25fc8-110">HTTP method</span></span> | <span data-ttu-id="25fc8-111">Względny identyfikator URI</span><span class="sxs-lookup"><span data-stu-id="25fc8-111">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="25fc8-112">Pobieranie produktu według Identyfikatora</span><span class="sxs-lookup"><span data-stu-id="25fc8-112">Get a product by ID</span></span> | <span data-ttu-id="25fc8-113">GET</span><span class="sxs-lookup"><span data-stu-id="25fc8-113">GET</span></span> | <span data-ttu-id="25fc8-114">/ InterfejsAPI/produkty/*identyfikator*</span><span class="sxs-lookup"><span data-stu-id="25fc8-114">/api/products/*id*</span></span> |
| <span data-ttu-id="25fc8-115">Tworzenie nowego produktu</span><span class="sxs-lookup"><span data-stu-id="25fc8-115">Create a new product</span></span> | <span data-ttu-id="25fc8-116">POST</span><span class="sxs-lookup"><span data-stu-id="25fc8-116">POST</span></span> | <span data-ttu-id="25fc8-117">/ api/produktów</span><span class="sxs-lookup"><span data-stu-id="25fc8-117">/api/products</span></span> |
| <span data-ttu-id="25fc8-118">Aktualizacja produktu</span><span class="sxs-lookup"><span data-stu-id="25fc8-118">Update a product</span></span> | <span data-ttu-id="25fc8-119">PUT</span><span class="sxs-lookup"><span data-stu-id="25fc8-119">PUT</span></span> | <span data-ttu-id="25fc8-120">/ InterfejsAPI/produkty/*identyfikator*</span><span class="sxs-lookup"><span data-stu-id="25fc8-120">/api/products/*id*</span></span> |
| <span data-ttu-id="25fc8-121">Usuwanie produktu</span><span class="sxs-lookup"><span data-stu-id="25fc8-121">Delete a product</span></span> | <span data-ttu-id="25fc8-122">DELETE</span><span class="sxs-lookup"><span data-stu-id="25fc8-122">DELETE</span></span> | <span data-ttu-id="25fc8-123">/ InterfejsAPI/produkty/*identyfikator*</span><span class="sxs-lookup"><span data-stu-id="25fc8-123">/api/products/*id*</span></span> |

<span data-ttu-id="25fc8-124">Aby dowiedzieć się, jak zaimplementować ten interfejs API za pomocą interfejsu API sieci Web platformy ASP.NET, zobacz [Tworzenie internetowego interfejsu API tej operacji CRUD obsługuje](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span><span class="sxs-lookup"><span data-stu-id="25fc8-124">To learn how to implement this API with ASP.NET Web API, see [Creating a Web API that Supports CRUD Operations](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span></span>

<span data-ttu-id="25fc8-125">Dla uproszczenia aplikacji klienckiej w tym samouczku jest aplikacją konsoli Windows.</span><span class="sxs-lookup"><span data-stu-id="25fc8-125">For simplicity, the client application in this tutorial is a Windows console application.</span></span> <span data-ttu-id="25fc8-126">**HttpClient** jest również obsługiwana dla aplikacji Windows Phone i Windows Store.</span><span class="sxs-lookup"><span data-stu-id="25fc8-126">**HttpClient** is also supported for Windows Phone and Windows Store apps.</span></span> <span data-ttu-id="25fc8-127">Aby uzyskać więcej informacji, zobacz [pisanie kodu klienta interfejsu API sieci Web dla wielu platform przy użyciu przenośnych bibliotek](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span><span class="sxs-lookup"><span data-stu-id="25fc8-127">For more information, see [Writing Web API Client Code for Multiple Platforms Using Portable Libraries](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span></span>

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a><span data-ttu-id="25fc8-128">Tworzenie aplikacji konsoli</span><span class="sxs-lookup"><span data-stu-id="25fc8-128">Create the Console Application</span></span>

<span data-ttu-id="25fc8-129">W programie Visual Studio Utwórz nową aplikację konsoli Windows o nazwie **HttpClientSample** i wklej następujący kod:</span><span class="sxs-lookup"><span data-stu-id="25fc8-129">In Visual Studio, create a new Windows console app named **HttpClientSample** and paste in the following code:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

<span data-ttu-id="25fc8-130">Powyższy kod jest aplikacją kliencką ukończone.</span><span class="sxs-lookup"><span data-stu-id="25fc8-130">The preceding code is the complete client app.</span></span>

<span data-ttu-id="25fc8-131">`RunAsync` Przebiegi i bloki przed zakończeniem tego procesu.</span><span class="sxs-lookup"><span data-stu-id="25fc8-131">`RunAsync` runs and blocks until it completes.</span></span> <span data-ttu-id="25fc8-132">Większość **HttpClient** metody są asynchroniczne, ponieważ wykonują we/wy sieci.</span><span class="sxs-lookup"><span data-stu-id="25fc8-132">Most **HttpClient** methods are async, because they perform network I/O.</span></span> <span data-ttu-id="25fc8-133">Wszystkie zadania asynchroniczne są wykonywane tylko wewnątrz `RunAsync`.</span><span class="sxs-lookup"><span data-stu-id="25fc8-133">All of the async tasks are done inside `RunAsync`.</span></span> <span data-ttu-id="25fc8-134">Zwykle aplikacja nie blokuje wątku głównego, ale ta aplikacja nie zezwala na interakcji.</span><span class="sxs-lookup"><span data-stu-id="25fc8-134">Normally an app doesn't block the main thread, but this app doesn't allow any interaction.</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a><span data-ttu-id="25fc8-135">Zainstaluj biblioteki klienta interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="25fc8-135">Install the Web API Client Libraries</span></span>

<span data-ttu-id="25fc8-136">Użyj Menedżera pakietów NuGet można zainstalować pakietu biblioteki klienta interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="25fc8-136">Use NuGet Package Manager to install the Web API Client Libraries package.</span></span>

<span data-ttu-id="25fc8-137">Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="25fc8-137">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="25fc8-138">W konsoli Menedżera pakietów (PMC), wpisz następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="25fc8-138">In the Package Manager Console (PMC), type the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.Client`

<span data-ttu-id="25fc8-139">Poprzednie polecenie dodaje następujące pakiety NuGet do projektu:</span><span class="sxs-lookup"><span data-stu-id="25fc8-139">The preceding command adds the following NuGet packages to the project:</span></span>

* <span data-ttu-id="25fc8-140">Microsoft.AspNet.WebApi.Client</span><span class="sxs-lookup"><span data-stu-id="25fc8-140">Microsoft.AspNet.WebApi.Client</span></span>
* <span data-ttu-id="25fc8-141">Newtonsoft.Json</span><span class="sxs-lookup"><span data-stu-id="25fc8-141">Newtonsoft.Json</span></span>

<span data-ttu-id="25fc8-142">Program Json.NET to popularne środowisko JSON o wysokiej wydajności dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="25fc8-142">Json.NET is a popular high-performance JSON framework for .NET.</span></span>

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a><span data-ttu-id="25fc8-143">Dodawanie klasy modelu</span><span class="sxs-lookup"><span data-stu-id="25fc8-143">Add a Model Class</span></span>

<span data-ttu-id="25fc8-144">Sprawdź `Product` klasy:</span><span class="sxs-lookup"><span data-stu-id="25fc8-144">Examine the `Product` class:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

<span data-ttu-id="25fc8-145">Ta klasa pasuje do modelu danych, używane przez internetowy interfejs API.</span><span class="sxs-lookup"><span data-stu-id="25fc8-145">This class matches the data model used by the web API.</span></span> <span data-ttu-id="25fc8-146">Aplikacja może używać **HttpClient** odczytać `Product` wystąpienie z odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="25fc8-146">An app can use **HttpClient** to read a `Product` instance from an HTTP response.</span></span> <span data-ttu-id="25fc8-147">Aplikacja nie ma do pisania kodu deserializacji.</span><span class="sxs-lookup"><span data-stu-id="25fc8-147">The app doesn't have to write any deserialization code.</span></span>

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a><span data-ttu-id="25fc8-148">Tworzenie i Inicjowanie klasy HttpClient</span><span class="sxs-lookup"><span data-stu-id="25fc8-148">Create and Initialize HttpClient</span></span>

<span data-ttu-id="25fc8-149">Sprawdź statycznej **HttpClient** właściwości:</span><span class="sxs-lookup"><span data-stu-id="25fc8-149">Examine the static **HttpClient** property:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

<span data-ttu-id="25fc8-150">**HttpClient** jest przeznaczony do jednorazowego utworzenia ich i ponownie używane w całym cyklu życia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="25fc8-150">**HttpClient** is intended to be instantiated once and reused throughout the life of an application.</span></span> <span data-ttu-id="25fc8-151">Następujące warunki mogą spowodować **SocketException** błędów:</span><span class="sxs-lookup"><span data-stu-id="25fc8-151">The following conditions can result in **SocketException** errors:</span></span>

* <span data-ttu-id="25fc8-152">Tworzenie nowego **HttpClient** wystąpienia na żądanie.</span><span class="sxs-lookup"><span data-stu-id="25fc8-152">Creating a new **HttpClient** instance per request.</span></span>
* <span data-ttu-id="25fc8-153">Serwer z dużym obciążeniem.</span><span class="sxs-lookup"><span data-stu-id="25fc8-153">Server under heavy load.</span></span>

<span data-ttu-id="25fc8-154">Tworzenie nowego **HttpClient** wystąpienia na żądanie może wyczerpać dostępne gniazda.</span><span class="sxs-lookup"><span data-stu-id="25fc8-154">Creating a new **HttpClient** instance per request can exhaust the available sockets.</span></span>

<span data-ttu-id="25fc8-155">Poniższy kod inicjuje **HttpClient** wystąpienie:</span><span class="sxs-lookup"><span data-stu-id="25fc8-155">The following code initializes the **HttpClient** instance:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

<span data-ttu-id="25fc8-156">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="25fc8-156">The preceding code:</span></span>

* <span data-ttu-id="25fc8-157">Ustawia podstawowy identyfikator URI dla żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="25fc8-157">Sets the base URI for HTTP requests.</span></span> <span data-ttu-id="25fc8-158">Zmień numer portu na port używany w aplikacji serwera.</span><span class="sxs-lookup"><span data-stu-id="25fc8-158">Change the port number to the port used in the server app.</span></span> <span data-ttu-id="25fc8-159">Aplikacja nie będzie działać, chyba że używany jest port dla aplikacji serwera.</span><span class="sxs-lookup"><span data-stu-id="25fc8-159">The app won't work unless port for the server app is used.</span></span>
* <span data-ttu-id="25fc8-160">Ustawia nagłówek Accept do "application/json".</span><span class="sxs-lookup"><span data-stu-id="25fc8-160">Sets the Accept header to "application/json".</span></span> <span data-ttu-id="25fc8-161">Ustawienie tego pliku nagłówkowego nakazuje serwerowi na wysyłanie danych w formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="25fc8-161">Setting this header tells the server to send data in JSON format.</span></span>

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a><span data-ttu-id="25fc8-162">Wyślij żądanie Pobierz do pobrania zasobu</span><span class="sxs-lookup"><span data-stu-id="25fc8-162">Send a GET request to retrieve a resource</span></span>

<span data-ttu-id="25fc8-163">Poniższy kod wysyła żądanie pobrania produktu:</span><span class="sxs-lookup"><span data-stu-id="25fc8-163">The following code sends a GET request for a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

<span data-ttu-id="25fc8-164">**GetAsync** metoda wysyła żądanie HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="25fc8-164">The **GetAsync** method sends the HTTP GET request.</span></span> <span data-ttu-id="25fc8-165">Po ukończeniu metody, zwraca **obiektu HttpResponseMessage** zawierający odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="25fc8-165">When the method completes, it returns an **HttpResponseMessage** that contains the HTTP response.</span></span> <span data-ttu-id="25fc8-166">Jeśli kod stanu w odpowiedzi jest kod powodzenia, treść odpowiedzi zawiera reprezentację JSON produktu.</span><span class="sxs-lookup"><span data-stu-id="25fc8-166">If the status code in the response is a success code, the response body contains the JSON representation of a product.</span></span> <span data-ttu-id="25fc8-167">Wywołaj **ReadAsAsync** do ładunku JSON do zdeserializowania `Product` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="25fc8-167">Call **ReadAsAsync** to deserialize the JSON payload to a `Product` instance.</span></span> <span data-ttu-id="25fc8-168">**ReadAsAsync** metoda jest asynchroniczne, ponieważ treść odpowiedzi mogą być dowolnie duże.</span><span class="sxs-lookup"><span data-stu-id="25fc8-168">The **ReadAsAsync** method is asynchronous because the response body can be arbitrarily large.</span></span>

<span data-ttu-id="25fc8-169">**HttpClient** nie zgłasza wyjątku, gdy odpowiedź HTTP zawiera kod błędu.</span><span class="sxs-lookup"><span data-stu-id="25fc8-169">**HttpClient** does not throw an exception when the HTTP response contains an error code.</span></span> <span data-ttu-id="25fc8-170">Zamiast tego **IsSuccessStatusCode** właściwość **false** Jeśli stan jest kod błędu.</span><span class="sxs-lookup"><span data-stu-id="25fc8-170">Instead, the **IsSuccessStatusCode** property is **false** if the status is an error code.</span></span> <span data-ttu-id="25fc8-171">Jeśli wolisz traktować kody błędów HTTP jako wyjątki, wywołania [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) na obiekcie odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="25fc8-171">If you prefer to treat HTTP error codes as exceptions, call [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) on the response object.</span></span> <span data-ttu-id="25fc8-172">`EnsureSuccessStatusCode` zgłasza wyjątek, jeśli kod stanu mieści się poza zakresem 200&ndash;299.</span><span class="sxs-lookup"><span data-stu-id="25fc8-172">`EnsureSuccessStatusCode` throws an exception if the status code falls outside the range 200&ndash;299.</span></span> <span data-ttu-id="25fc8-173">Należy pamiętać, że **HttpClient** może generować wyjątki z innych przyczyn &mdash; na przykład, jeśli upłynie limit czasu żądania.</span><span class="sxs-lookup"><span data-stu-id="25fc8-173">Note that **HttpClient** can throw exceptions for other reasons &mdash; for example, if the request times out.</span></span>

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a><span data-ttu-id="25fc8-174">Programy formatujące typy nośnika do deserializacji</span><span class="sxs-lookup"><span data-stu-id="25fc8-174">Media-Type Formatters to Deserialize</span></span>

<span data-ttu-id="25fc8-175">Gdy **ReadAsAsync** jest wywoływana bez parametrów, używa domyślny zestaw *programy formatujące multimedia* do odczytu treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="25fc8-175">When **ReadAsAsync** is called with no parameters, it uses a default set of *media formatters* to read the response body.</span></span> <span data-ttu-id="25fc8-176">Domyślne elementy formatujące obsługi formatu JSON, XML i dane zakodowane jako adres url formularza.</span><span class="sxs-lookup"><span data-stu-id="25fc8-176">The default formatters support JSON, XML, and Form-url-encoded data.</span></span>

<span data-ttu-id="25fc8-177">Zamiast używania elementy formatujące domyślne, możesz podać listę programów formatujących do **ReadAsAsync** metody.</span><span class="sxs-lookup"><span data-stu-id="25fc8-177">Instead of using the default formatters, you can provide a list of formatters to the **ReadAsAsync** method.</span></span>  <span data-ttu-id="25fc8-178">Korzystanie z listy elementów formatujących jest przydatna, jeśli element formatujący typu nośnika niestandardowego:</span><span class="sxs-lookup"><span data-stu-id="25fc8-178">Using a list of formatters is useful if you have a custom media-type formatter:</span></span>

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

<span data-ttu-id="25fc8-179">Aby uzyskać więcej informacji, zobacz [programy formatujące multimedia w programie ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)</span><span class="sxs-lookup"><span data-stu-id="25fc8-179">For more information, see [Media Formatters in ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)</span></span>

## <a name="sending-a-post-request-to-create-a-resource"></a><span data-ttu-id="25fc8-180">Wysłanie żądania POST do utworzenia zasobu</span><span class="sxs-lookup"><span data-stu-id="25fc8-180">Sending a POST Request to Create a Resource</span></span>

<span data-ttu-id="25fc8-181">Poniższy kod wysyła żądanie POST, która zawiera `Product` wystąpienia w formacie JSON:</span><span class="sxs-lookup"><span data-stu-id="25fc8-181">The following code sends a POST request that contains a `Product` instance in JSON format:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

<span data-ttu-id="25fc8-182">**PostAsJsonAsync** metody:</span><span class="sxs-lookup"><span data-stu-id="25fc8-182">The **PostAsJsonAsync** method:</span></span>

* <span data-ttu-id="25fc8-183">Serializuje obiekt do formatu JSON.</span><span class="sxs-lookup"><span data-stu-id="25fc8-183">Serializes an object to JSON.</span></span>
* <span data-ttu-id="25fc8-184">Wysyła żądanie POST ładunek JSON.</span><span class="sxs-lookup"><span data-stu-id="25fc8-184">Sends the JSON payload in a POST request.</span></span>

<span data-ttu-id="25fc8-185">Jeśli żądanie zakończy się powodzeniem:</span><span class="sxs-lookup"><span data-stu-id="25fc8-185">If the request succeeds:</span></span>

* <span data-ttu-id="25fc8-186">Powinien on zwrócić odpowiedź 201 (utworzono).</span><span class="sxs-lookup"><span data-stu-id="25fc8-186">It should return a 201 (Created) response.</span></span>
* <span data-ttu-id="25fc8-187">Odpowiedź powinna zawierać adres URL zasobów utworzonych w nagłówku Location.</span><span class="sxs-lookup"><span data-stu-id="25fc8-187">The response should include the URL of the created resources in the Location header.</span></span>

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a><span data-ttu-id="25fc8-188">Wysyła żądanie PUT, aby zaktualizować zasobu</span><span class="sxs-lookup"><span data-stu-id="25fc8-188">Sending a PUT Request to Update a Resource</span></span>

<span data-ttu-id="25fc8-189">Poniższy kod wysyła żądanie PUT do aktualizacji produktu:</span><span class="sxs-lookup"><span data-stu-id="25fc8-189">The following code sends a PUT request to update a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

<span data-ttu-id="25fc8-190">**PutAsJsonAsync** metoda działa jak **PostAsJsonAsync**, z tą różnicą, że wysyła żądanie PUT zamiast wpisu.</span><span class="sxs-lookup"><span data-stu-id="25fc8-190">The **PutAsJsonAsync** method works like **PostAsJsonAsync**, except that it sends a PUT request instead of POST.</span></span>

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a><span data-ttu-id="25fc8-191">Trwa wysyłanie żądania usunięcia, można usunąć zasobu</span><span class="sxs-lookup"><span data-stu-id="25fc8-191">Sending a DELETE Request to Delete a Resource</span></span>

<span data-ttu-id="25fc8-192">Poniższy kod wysyła żądanie usunięcia, można usunąć produktu:</span><span class="sxs-lookup"><span data-stu-id="25fc8-192">The following code sends a DELETE request to delete a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

<span data-ttu-id="25fc8-193">Żądanie usunięcia takich jak GET, nie ma treści żądania.</span><span class="sxs-lookup"><span data-stu-id="25fc8-193">Like GET, a DELETE request does not have a request body.</span></span> <span data-ttu-id="25fc8-194">Nie potrzebujesz określić format JSON lub XML za pomocą usuwania.</span><span class="sxs-lookup"><span data-stu-id="25fc8-194">You don't need to specify JSON or XML format with DELETE.</span></span>

## <a name="test-the-sample"></a><span data-ttu-id="25fc8-195">Test próbki</span><span class="sxs-lookup"><span data-stu-id="25fc8-195">Test the sample</span></span>

<span data-ttu-id="25fc8-196">Aby przetestować aplikację klienta:</span><span class="sxs-lookup"><span data-stu-id="25fc8-196">To test the client app:</span></span>

1. <span data-ttu-id="25fc8-197">[Pobierz](https://github.com/aspnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) i uruchamianie aplikacji serwera.</span><span class="sxs-lookup"><span data-stu-id="25fc8-197">[Download](https://github.com/aspnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) and run the server app.</span></span> <span data-ttu-id="25fc8-198">[Instrukcje pobierania](/aspnet/core/tutorials/#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="25fc8-198">[Download instructions](/aspnet/core/tutorials/#how-to-download-a-sample).</span></span> <span data-ttu-id="25fc8-199">Sprawdź, czy działa aplikacja serwera.</span><span class="sxs-lookup"><span data-stu-id="25fc8-199">Verify the server app is working.</span></span> <span data-ttu-id="25fc8-200">Na przykład `http://localhost:64195/api/products` powinna zwrócić listę produktów.</span><span class="sxs-lookup"><span data-stu-id="25fc8-200">For example, `http://localhost:64195/api/products` should return a list of products.</span></span>
2. <span data-ttu-id="25fc8-201">Ustaw podstawowy identyfikator URI dla żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="25fc8-201">Set the base URI for HTTP requests.</span></span> <span data-ttu-id="25fc8-202">Zmień numer portu na port używany w aplikacji serwera.</span><span class="sxs-lookup"><span data-stu-id="25fc8-202">Change the port number to the port used in the server app.</span></span>
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. <span data-ttu-id="25fc8-203">Uruchom aplikację klienta.</span><span class="sxs-lookup"><span data-stu-id="25fc8-203">Run the client app.</span></span> <span data-ttu-id="25fc8-204">Są następujące wyniki:</span><span class="sxs-lookup"><span data-stu-id="25fc8-204">The following output is produced:</span></span>

   ```console
   Created at http://localhost:64195/api/products/4
   Name: Gizmo     Price: 100.0    Category: Widgets
   Updating price...
   Name: Gizmo     Price: 80.0     Category: Widgets
   Deleted (HTTP Status = 204)
   ```
