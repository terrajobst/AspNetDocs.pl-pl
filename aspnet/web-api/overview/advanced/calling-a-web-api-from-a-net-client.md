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
ms.openlocfilehash: 960960d26863cc3f725eee8a6c98844c5d3ce721
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519183"
---
# <a name="call-a-web-api-from-a-net-client-c"></a>Wywoływanie internetowego interfejsu API z klienta platformy .NETC#()

według [Jan Wasson](https://github.com/MikeWasson) i [Rick Anderson](https://twitter.com/RickAndMSFT)

[Pobierz ukończony projekt](https://github.com/aspnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample). [Instrukcje pobierania](/aspnet/core/tutorials/#how-to-download-a-sample). 

W tym samouczku pokazano, jak wywołać interfejs API sieci Web z aplikacji .NET przy użyciu [systemu .NET. http. HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)

W tym samouczku aplikacja kliencka jest zapisywana, która wykorzystuje następujący internetowy interfejs API:

| Akcja | Metoda HTTP | Względny identyfikator URI |
| --- | --- | --- |
| Pobierz produkt według identyfikatora | GET | *Identyfikator* /API/Products/ |
| Utwórz nowy produkt | POST | /api/products |
| Aktualizowanie produktu | PUT | *Identyfikator* /API/Products/ |
| Usuwanie produktu | DELETE | *Identyfikator* /API/Products/ |

Aby dowiedzieć się, jak zaimplementować ten interfejs API za pomocą interfejsu API sieci Web ASP.NET, zobacz [Tworzenie internetowego interfejsu API obsługującego operacje CRUD](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).

Dla uproszczenia aplikacja kliencka w tym samouczku jest aplikacją konsolową dla systemu Windows. **HttpClient** jest również obsługiwana dla Windows Phone i aplikacji ze sklepu Windows. Aby uzyskać więcej informacji, zobacz [pisanie kodu klienta interfejsu API sieci Web dla wielu platform przy użyciu bibliotek przenośnych](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a>Tworzenie aplikacji konsolowej

W programie Visual Studio Utwórz nową aplikację konsolową systemu Windows o nazwie **HttpClientSample** i wklej ją w następującym kodzie:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

Poprzedni kod jest kompletną aplikacją kliencką.

`RunAsync` działa i blokuje do momentu ukończenia. Większość metod **HttpClient** jest asynchroniczna, ponieważ wykonują we/wy sieci. Wszystkie zadania asynchroniczne są wykonywane w `RunAsync`. Zwykle aplikacja nie blokuje wątku głównego, ale ta aplikacja nie zezwala na interakcję.

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a>Instalowanie bibliotek klienckich interfejsu API sieci Web

Zainstaluj pakiet biblioteki klienta internetowego interfejsu API za pomocą Menedżera pakietów NuGet.

Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów**. W konsoli Menedżera pakietów (PMC) wpisz następujące polecenie:

`Install-Package Microsoft.AspNet.WebApi.Client`

Poprzednie polecenie dodaje następujące pakiety NuGet do projektu:

* Microsoft.AspNet.WebApi.Client
* Newtonsoft.Json

Netwonsoft. JSON (znany również jako Json.NET) to popularne środowisko JSON o wysokiej wydajności dla platformy .NET.

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a>Dodaj klasę modelu

Zapoznaj się z klasą `Product`:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

Ta klasa jest zgodna z modelem danych używanym przez internetowy interfejs API. Aplikacja może używać **HttpClient** , aby odczytywać wystąpienie `Product` z odpowiedzi HTTP. Aplikacja nie musi pisać kodu deserializacji.

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a>Utwórz i zainicjuj HttpClient

Przejrzyj statyczną Właściwość **HttpClient** :

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

**HttpClient** jest przeznaczony do tworzenia wystąpień i ponownie używany przez cały czas życia aplikacji. Następujące warunki mogą spowodować błędy **gniazdaexception** :

* Tworzenie nowego wystąpienia **HttpClient** na żądanie.
* Serwer z dużym obciążeniem.

Utworzenie nowego wystąpienia **HttpClient** na żądanie może obsłużyć dostępne gniazda.

Poniższy kod Inicjuje wystąpienie **HttpClient** :

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

Powyższy kod:

* Ustawia podstawowy identyfikator URI dla żądań HTTP. Zmień numer portu na port używany w aplikacji serwera. Aplikacja nie będzie działała, chyba że zostanie użyty port dla aplikacji serwera.
* Ustawia nagłówek Accept na wartość "Application/JSON". Ustawienie tego nagłówka nakazuje serwerowi wysyłanie danych w formacie JSON.

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a>Wysyłanie żądania GET w celu pobrania zasobu

Poniższy kod wysyła żądanie GET dla produktu:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

Metoda **GetAsync** wysyła żądanie HTTP GET. Gdy metoda zostanie ukończona, zwraca **HttpResponseMessage** , która zawiera odpowiedź HTTP. Jeśli kod stanu w odpowiedzi to kod sukcesu, treść odpowiedzi zawiera reprezentację w formacie JSON produktu. Wywołaj **ReadAsAsync** , aby ZDESERIALIZOWAĆ ładunek JSON do wystąpienia `Product`. Metoda **ReadAsAsync** jest asynchroniczna, ponieważ treść odpowiedzi może być arbitralnie duża.

**HttpClient** nie zgłasza wyjątku, gdy odpowiedź HTTP zawiera kod błędu. Zamiast tego Właściwość **IsSuccessStatusCode** ma **wartość false** , jeśli stan jest kodem błędu. Jeśli wolisz traktować kody błędów HTTP jako wyjątki, wywołaj [HttpResponseMessage. EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) w obiekcie Response. `EnsureSuccessStatusCode` zgłasza wyjątek, jeśli kod stanu znajduje się poza zakresem 200&ndash;299. Należy pamiętać, że **HttpClient** może zgłosić wyjątki z innych przyczyn &mdash; na przykład, jeśli limit czasu żądania upłynął.

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a>Elementy formatujące typu multimediów do deserializacji

Gdy **ReadAsAsync** jest wywoływana bez parametrów, używa domyślnego zestawu elementów *formatujących multimedia* do odczytywania treści odpowiedzi. Domyślne elementy formatujące obsługują dane JSON, XML i zakodowane w adresie URL.

Zamiast korzystać z domyślnych elementów formatujących, można dostarczyć listę elementów formatujących do metody **ReadAsAsync** .  Korzystanie z listy elementów formatujących jest przydatne, jeśli masz niestandardowy program formatujący typ nośnika:

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

Aby uzyskać więcej informacji, zobacz elementy [formatujące multimedia w ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)

## <a name="sending-a-post-request-to-create-a-resource"></a>Wysyłanie żądania POST w celu utworzenia zasobu

Poniższy kod wysyła żądanie POST zawierające wystąpienie `Product` w formacie JSON:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

Metoda **PostAsJsonAsync** :

* Serializacja obiektu do formatu JSON.
* Wysyła ładunek JSON w żądaniu POST.

Jeśli żądanie zakończy się pomyślnie:

* Powinna zwrócić odpowiedź 201 (utworzona).
* Odpowiedź powinna zawierać adres URL utworzonych zasobów w nagłówku lokalizacji.

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a>Wysyłanie żądania PUT w celu zaktualizowania zasobu

Poniższy kod wysyła żądanie PUT w celu zaktualizowania produktu:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

Metoda **PutAsJsonAsync** działa podobnie jak **PostAsJsonAsync**, z tą różnicą, że wysyła żądanie Put zamiast post.

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a>Wysyłanie żądania usunięcia w celu usunięcia zasobu

Poniższy kod wysyła żądanie usunięcia, aby usunąć produkt:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

Podobnie jak pobieranie, żądanie usunięcia nie ma treści żądania. Nie musisz określać formatu JSON ani XML z opcją DELETE.

## <a name="test-the-sample"></a>Testowanie przykładu

Aby przetestować aplikację kliencką:

1. [Pobierz](https://github.com/aspnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) i uruchom aplikację serwera. [Instrukcje pobierania](/aspnet/core/#how-to-download-a-sample). Sprawdź, czy aplikacja serwera działa. Na przykład `http://localhost:64195/api/products` powinna zwrócić listę produktów.
2. Ustaw podstawowy identyfikator URI dla żądań HTTP. Zmień numer portu na port używany w aplikacji serwera.
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. Uruchom aplikację kliencką. Zostaną wyświetlone następujące dane wyjściowe:

   ```console
   Created at http://localhost:64195/api/products/4
   Name: Gizmo     Price: 100.0    Category: Widgets
   Updating price...
   Name: Gizmo     Price: 80.0     Category: Widgets
   Deleted (HTTP Status = 204)
   ```
