---
uid: web-api/overview/advanced/http-cookies
title: Pliki cookie protokołu HTTP w interfejsie Web API ASP.NET — ASP.NET 4. x
author: MikeWasson
description: Opisuje sposób wysyłania i odbierania plików cookie protokołu HTTP w interfejsie Web API dla ASP.NET 4. x.
ms.author: riande
ms.date: 09/17/2012
ms.custom: seoapril2019
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: 8ca26ff6776daa13bc4f8b06c2eba61afcfefba2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557688"
---
# <a name="http-cookies-in-aspnet-web-api"></a><span data-ttu-id="7e066-103">Pliki cookie protokołu HTTP we wzorcu ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="7e066-103">HTTP Cookies in ASP.NET Web API</span></span>

<span data-ttu-id="7e066-104">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="7e066-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="7e066-105">W tym temacie opisano sposób wysyłania i odbierania plików cookie protokołu HTTP w interfejsie API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="7e066-105">This topic describes how to send and receive HTTP cookies in Web API.</span></span>

## <a name="background-on-http-cookies"></a><span data-ttu-id="7e066-106">Tło dla plików cookie HTTP</span><span class="sxs-lookup"><span data-stu-id="7e066-106">Background on HTTP Cookies</span></span>

<span data-ttu-id="7e066-107">Ta sekcja zawiera krótkie omówienie sposobu implementacji plików cookie na poziomie protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="7e066-107">This section gives a brief overview of how cookies are implemented at the HTTP level.</span></span> <span data-ttu-id="7e066-108">Aby uzyskać szczegółowe informacje, zapoznaj się z [dokumentem RFC 6265](http://tools.ietf.org/html/rfc6265).</span><span class="sxs-lookup"><span data-stu-id="7e066-108">For details, consult [RFC 6265](http://tools.ietf.org/html/rfc6265).</span></span>

<span data-ttu-id="7e066-109">Plik cookie to element danych wysyłany przez serwer w odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="7e066-109">A cookie is a piece of data that a server sends in the HTTP response.</span></span> <span data-ttu-id="7e066-110">Klient (opcjonalnie) przechowuje plik cookie i zwraca go na kolejne żądania.</span><span class="sxs-lookup"><span data-stu-id="7e066-110">The client (optionally) stores the cookie and returns it on subsequent requests.</span></span> <span data-ttu-id="7e066-111">Umożliwia to klientowi i serwerowi udostępnianie stanu.</span><span class="sxs-lookup"><span data-stu-id="7e066-111">This allows the client and server to share state.</span></span> <span data-ttu-id="7e066-112">Aby ustawić plik cookie, serwer zawiera nagłówek Set-cookie w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="7e066-112">To set a cookie, the server includes a Set-Cookie header in the response.</span></span> <span data-ttu-id="7e066-113">Format pliku cookie to para nazwa-wartość z opcjonalnymi atrybutami.</span><span class="sxs-lookup"><span data-stu-id="7e066-113">The format of a cookie is a name-value pair, with optional attributes.</span></span> <span data-ttu-id="7e066-114">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="7e066-114">For example:</span></span>

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

<span data-ttu-id="7e066-115">Oto przykład z atrybutami:</span><span class="sxs-lookup"><span data-stu-id="7e066-115">Here is an example with attributes:</span></span>

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

<span data-ttu-id="7e066-116">Aby zwrócić plik cookie na serwer, klient zawiera nagłówek pliku cookie w późniejszych żądaniach.</span><span class="sxs-lookup"><span data-stu-id="7e066-116">To return a cookie to the server, the client includes a Cookie header in later requests.</span></span>

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

<span data-ttu-id="7e066-117">Odpowiedź HTTP może zawierać wiele nagłówków Set-cookie.</span><span class="sxs-lookup"><span data-stu-id="7e066-117">An HTTP response can include multiple Set-Cookie headers.</span></span>

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

<span data-ttu-id="7e066-118">Klient zwraca wiele plików cookie przy użyciu jednego nagłówka pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="7e066-118">The client returns multiple cookies using a single Cookie header.</span></span>

[!code-console[Main](http-cookies/samples/sample5.cmd)]

<span data-ttu-id="7e066-119">Zakres i czas trwania pliku cookie są kontrolowane przez następujące atrybuty w nagłówku Set-Cookie:</span><span class="sxs-lookup"><span data-stu-id="7e066-119">The scope and duration of a cookie are controlled by following attributes in the Set-Cookie header:</span></span>

- <span data-ttu-id="7e066-120">**Domena**: informuje klienta, która domena powinna odebrać plik cookie.</span><span class="sxs-lookup"><span data-stu-id="7e066-120">**Domain**: Tells the client which domain should receive the cookie.</span></span> <span data-ttu-id="7e066-121">Na przykład jeśli domena jest "example.com", klient zwraca plik cookie do każdej poddomeny example.com.</span><span class="sxs-lookup"><span data-stu-id="7e066-121">For example, if the domain is "example.com", the client returns the cookie to every subdomain of example.com.</span></span> <span data-ttu-id="7e066-122">Jeśli nie zostanie określony, domena jest serwerem pochodzenia.</span><span class="sxs-lookup"><span data-stu-id="7e066-122">If not specified, the domain is the origin server.</span></span>
- <span data-ttu-id="7e066-123">**Ścieżka**: ogranicza plik cookie do określonej ścieżki w domenie.</span><span class="sxs-lookup"><span data-stu-id="7e066-123">**Path**: Restricts the cookie to the specified path within the domain.</span></span> <span data-ttu-id="7e066-124">Jeśli nie zostanie określony, zostanie użyta ścieżka identyfikatora URI żądania.</span><span class="sxs-lookup"><span data-stu-id="7e066-124">If not specified, the path of the request URI is used.</span></span>
- <span data-ttu-id="7e066-125">**Wygasa**: ustawia datę wygaśnięcia pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="7e066-125">**Expires**: Sets an expiration date for the cookie.</span></span> <span data-ttu-id="7e066-126">Klient usuwa plik cookie po jego wygaśnięciu.</span><span class="sxs-lookup"><span data-stu-id="7e066-126">The client deletes the cookie when it expires.</span></span>
- <span data-ttu-id="7e066-127">**Max-age**: ustawia maksymalny wiek pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="7e066-127">**Max-Age**: Sets the maximum age for the cookie.</span></span> <span data-ttu-id="7e066-128">Klient usuwa plik cookie po osiągnięciu maksymalnego wieku.</span><span class="sxs-lookup"><span data-stu-id="7e066-128">The client deletes the cookie when it reaches the maximum age.</span></span>

<span data-ttu-id="7e066-129">Jeśli ustawiono zarówno `Expires`, jak i `Max-Age`, `Max-Age` ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="7e066-129">If both `Expires` and `Max-Age` are set, `Max-Age` takes precedence.</span></span> <span data-ttu-id="7e066-130">Jeśli żadna z tych wartości nie zostanie ustawiona, klient usunie plik cookie po zakończeniu bieżącej sesji.</span><span class="sxs-lookup"><span data-stu-id="7e066-130">If neither is set, the client deletes the cookie when the current session ends.</span></span> <span data-ttu-id="7e066-131">(Dokładne znaczenie "Session" jest określane przez agenta użytkownika.)</span><span class="sxs-lookup"><span data-stu-id="7e066-131">(The exact meaning of "session" is determined by the user-agent.)</span></span>

<span data-ttu-id="7e066-132">Należy jednak pamiętać, że klienci mogą ignorować pliki cookie.</span><span class="sxs-lookup"><span data-stu-id="7e066-132">However, be aware that clients may ignore cookies.</span></span> <span data-ttu-id="7e066-133">Na przykład użytkownik może wyłączyć pliki cookie ze względów zachowania poufności informacji.</span><span class="sxs-lookup"><span data-stu-id="7e066-133">For example, a user might disable cookies for privacy reasons.</span></span> <span data-ttu-id="7e066-134">Klienci mogą usuwać pliki cookie przed ich wygaśnięciem lub ograniczyć liczbę przechowywanych plików cookie.</span><span class="sxs-lookup"><span data-stu-id="7e066-134">Clients may delete cookies before they expire, or limit the number of cookies stored.</span></span> <span data-ttu-id="7e066-135">Ze względu na prywatność klienci często odrzucają pliki cookie "inne firmy", w których domena nie jest zgodna z serwerem źródłowym.</span><span class="sxs-lookup"><span data-stu-id="7e066-135">For privacy reasons, clients often reject "third party" cookies, where the domain does not match the origin server.</span></span> <span data-ttu-id="7e066-136">W krótkim przypadku serwer nie powinien opierać się na przywracaniu plików cookie, które ustawia.</span><span class="sxs-lookup"><span data-stu-id="7e066-136">In short, the server should not rely on getting back the cookies that it sets.</span></span>

## <a name="cookies-in-web-api"></a><span data-ttu-id="7e066-137">Pliki cookie w internetowym interfejsie API</span><span class="sxs-lookup"><span data-stu-id="7e066-137">Cookies in Web API</span></span>

<span data-ttu-id="7e066-138">Aby dodać plik cookie do odpowiedzi HTTP, Utwórz wystąpienie **CookieHeaderValue** , które reprezentuje plik cookie.</span><span class="sxs-lookup"><span data-stu-id="7e066-138">To add a cookie to an HTTP response, create a **CookieHeaderValue** instance that represents the cookie.</span></span> <span data-ttu-id="7e066-139">Następnie Wywołaj metodę rozszerzenia **Addcookiess** , która jest zdefiniowana w **systemie .NET. http. HttpResponseHeadersExtensions** , aby dodać plik cookie.</span><span class="sxs-lookup"><span data-stu-id="7e066-139">Then call the **AddCookies** extension method, which is defined in the **System.Net.Http. HttpResponseHeadersExtensions** class, to add the cookie.</span></span>

<span data-ttu-id="7e066-140">Na przykład poniższy kod dodaje plik cookie w ramach akcji kontrolera:</span><span class="sxs-lookup"><span data-stu-id="7e066-140">For example, the following code adds a cookie within a controller action:</span></span>

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

<span data-ttu-id="7e066-141">Zwróć uwagę, że **addcookies** pobiera tablicę wystąpień **CookieHeaderValue** .</span><span class="sxs-lookup"><span data-stu-id="7e066-141">Notice that **AddCookies** takes an array of **CookieHeaderValue** instances.</span></span>

<span data-ttu-id="7e066-142">Aby wyodrębnić pliki cookie z żądania klienta, wywołaj metodę **Getcookiess** :</span><span class="sxs-lookup"><span data-stu-id="7e066-142">To extract the cookies from a client request, call the **GetCookies** method:</span></span>

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

<span data-ttu-id="7e066-143">**CookieHeaderValue** zawiera kolekcję wystąpień **CookieState** .</span><span class="sxs-lookup"><span data-stu-id="7e066-143">A **CookieHeaderValue** contains a collection of **CookieState** instances.</span></span> <span data-ttu-id="7e066-144">Każdy **CookieState** reprezentuje jeden plik cookie.</span><span class="sxs-lookup"><span data-stu-id="7e066-144">Each **CookieState** represents one cookie.</span></span> <span data-ttu-id="7e066-145">Użyj metody Indexer, aby uzyskać **CookieState** według nazwy, jak pokazano.</span><span class="sxs-lookup"><span data-stu-id="7e066-145">Use the indexer method to get a **CookieState** by name, as shown.</span></span>

## <a name="structured-cookie-data"></a><span data-ttu-id="7e066-146">Strukturalne dane pliku cookie</span><span class="sxs-lookup"><span data-stu-id="7e066-146">Structured Cookie Data</span></span>

<span data-ttu-id="7e066-147">Wiele przeglądarek ogranicza liczbę plików cookie, które będą&#8212;przechowywać zarówno całkowitą liczbę, jak i liczbę dla każdej domeny.</span><span class="sxs-lookup"><span data-stu-id="7e066-147">Many browsers limit how many cookies they will store&#8212;both the total number, and the number per domain.</span></span> <span data-ttu-id="7e066-148">W związku z tym może być przydatne umieszczenie danych strukturalnych w jednym pliku cookie zamiast ustawiania wielu plików cookie.</span><span class="sxs-lookup"><span data-stu-id="7e066-148">Therefore, it can be useful to put structured data into a single cookie, instead of setting multiple cookies.</span></span>

> [!NOTE]
> <span data-ttu-id="7e066-149">Specyfikacja RFC 6265 nie definiuje struktury danych plików cookie.</span><span class="sxs-lookup"><span data-stu-id="7e066-149">RFC 6265 does not define the structure of cookie data.</span></span>

<span data-ttu-id="7e066-150">Korzystając z klasy **CookieHeaderValue** , można przekazać listę par nazwa-wartość dla danych plików cookie.</span><span class="sxs-lookup"><span data-stu-id="7e066-150">Using the **CookieHeaderValue** class, you can pass a list of name-value pairs for the cookie data.</span></span> <span data-ttu-id="7e066-151">Te pary nazwa-wartość są kodowane jako dane formularza kodowane przy użyciu adresu URL w nagłówku Set-Cookie:</span><span class="sxs-lookup"><span data-stu-id="7e066-151">These name-value pairs are encoded as URL-encoded form data in the Set-Cookie header:</span></span>

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

<span data-ttu-id="7e066-152">Poprzedni kod generuje następujący nagłówek-cookie:</span><span class="sxs-lookup"><span data-stu-id="7e066-152">The previous code produces the following Set-Cookie header:</span></span>

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

<span data-ttu-id="7e066-153">Klasa **CookieState** zapewnia metodę indeksatora, która odczytuje wartości podrzędne z pliku cookie w komunikacie żądania:</span><span class="sxs-lookup"><span data-stu-id="7e066-153">The **CookieState** class provides an indexer method to read the sub-values from a cookie in the request message:</span></span>

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a><span data-ttu-id="7e066-154">Przykład: Ustawianie i pobieranie plików cookie w programie obsługi komunikatów</span><span class="sxs-lookup"><span data-stu-id="7e066-154">Example: Set and Retrieve Cookies in a Message Handler</span></span>

<span data-ttu-id="7e066-155">W poprzednich przykładach pokazano, jak używać plików cookie z poziomu kontrolera interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="7e066-155">The previous examples showed how to use cookies from within a Web API controller.</span></span> <span data-ttu-id="7e066-156">Innym rozwiązaniem jest użycie [obsługi komunikatów](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="7e066-156">Another option is to use [message handlers](http-message-handlers.md).</span></span> <span data-ttu-id="7e066-157">Procedury obsługi komunikatów są wywoływane wcześniej w potoku niż kontrolery.</span><span class="sxs-lookup"><span data-stu-id="7e066-157">Message handlers are invoked earlier in the pipeline than controllers.</span></span> <span data-ttu-id="7e066-158">Program obsługi komunikatów może odczytywać pliki cookie z żądania, zanim żądanie osiągnie kontroler lub doda pliki cookie do odpowiedzi po wygenerowaniu przez kontroler odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="7e066-158">A message handler can read cookies from the request before the request reaches the controller, or add cookies to the response after the controller generates the response.</span></span>

![](http-cookies/_static/image2.png)

<span data-ttu-id="7e066-159">Poniższy kod przedstawia procedurę obsługi komunikatów na potrzeby tworzenia identyfikatorów sesji.</span><span class="sxs-lookup"><span data-stu-id="7e066-159">The following code shows a message handler for creating session IDs.</span></span> <span data-ttu-id="7e066-160">Identyfikator sesji jest przechowywany w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="7e066-160">The session ID is stored in a cookie.</span></span> <span data-ttu-id="7e066-161">Program obsługi sprawdza żądanie dotyczące pliku cookie sesji.</span><span class="sxs-lookup"><span data-stu-id="7e066-161">The handler checks the request for the session cookie.</span></span> <span data-ttu-id="7e066-162">Jeśli żądanie nie zawiera pliku cookie, program obsługi generuje nowy identyfikator sesji.</span><span class="sxs-lookup"><span data-stu-id="7e066-162">If the request does not include the cookie, the handler generates a new session ID.</span></span> <span data-ttu-id="7e066-163">W obu przypadkach program obsługi przechowuje identyfikator sesji w zbiorze właściwości **HttpRequestMessage. Properties** .</span><span class="sxs-lookup"><span data-stu-id="7e066-163">In either case, the handler stores the session ID in the **HttpRequestMessage.Properties** property bag.</span></span> <span data-ttu-id="7e066-164">Dodaje również plik cookie sesji do odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="7e066-164">It also adds the session cookie to the HTTP response.</span></span>

<span data-ttu-id="7e066-165">Ta implementacja nie sprawdza, czy identyfikator sesji klienta został faktycznie wystawiony przez serwer.</span><span class="sxs-lookup"><span data-stu-id="7e066-165">This implementation does not validate that the session ID from the client was actually issued by the server.</span></span> <span data-ttu-id="7e066-166">Nie używaj go jako formy uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="7e066-166">Don't use it as a form of authentication!</span></span> <span data-ttu-id="7e066-167">W punkcie przykładu można wyświetlić zarządzanie plikami cookie HTTP.</span><span class="sxs-lookup"><span data-stu-id="7e066-167">The point of the example is to show HTTP cookie management.</span></span>

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

<span data-ttu-id="7e066-168">Kontroler może uzyskać identyfikator sesji z zbioru właściwości **HttpRequestMessage. Properties** .</span><span class="sxs-lookup"><span data-stu-id="7e066-168">A controller can get the session ID from the **HttpRequestMessage.Properties** property bag.</span></span>

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
