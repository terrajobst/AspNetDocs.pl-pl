---
uid: web-pages/overview/api-reference/asp-net-web-pages-api-reference
title: Krótkie informacje o interfejsie API ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: Ta strona zawiera listę z krótkimi przykładami najczęściej używanych obiektów, właściwościami i metodami programowania ASP.NET stron sieci Web za pomocą składnia Razor.
ms.author: riande
ms.date: 02/10/2014
ms.assetid: 4001cb9b-3bfd-4ace-8a89-1561d8421e2c
msc.legacyurl: /web-pages/overview/api-reference/asp-net-web-pages-api-reference
msc.type: authoredcontent
ms.openlocfilehash: e010307fc0576e8b003fbfe665cae77618d9c9a5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78574334"
---
# <a name="aspnet-web-pages-razor-api-quick-reference"></a><span data-ttu-id="61246-103">Krótkie informacje o interfejsie API ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="61246-103">ASP.NET Web Pages (Razor) API Quick Reference</span></span>

<span data-ttu-id="61246-104">Autor [FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="61246-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="61246-105">Ta strona zawiera listę z krótkimi przykładami najczęściej używanych obiektów, właściwościami i metodami programowania ASP.NET stron sieci Web za pomocą składnia Razor.</span><span class="sxs-lookup"><span data-stu-id="61246-105">This page contains a list with brief examples of the most commonly used objects, properties, and methods for programming ASP.NET Web Pages with Razor syntax.</span></span>
> 
> <span data-ttu-id="61246-106">Opisy oznaczone jako "(v2)" zostały wprowadzone w witrynie sieci Web ASP.NET w wersji 2.</span><span class="sxs-lookup"><span data-stu-id="61246-106">Descriptions marked with "(v2)" were introduced in ASP.NET Web Pages version 2.</span></span>
> 
> <span data-ttu-id="61246-107">Aby uzyskać dokumentację referencyjną interfejsu API, zobacz dokumentację dotyczącą [stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=208659) w witrynie MSDN.</span><span class="sxs-lookup"><span data-stu-id="61246-107">For API reference documentation, see the [ASP.NET Web Pages Reference Documentation](https://go.microsoft.com/fwlink/?LinkId=208659) on MSDN.</span></span>
> 
> ## <a name="software-versions"></a><span data-ttu-id="61246-108">Wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="61246-108">Software versions</span></span>
> 
> 
> - <span data-ttu-id="61246-109">ASP.NET strony sieci Web (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="61246-109">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="61246-110">Ten samouczek działa również z ASP.NET Web Pages 2 i ASP.NET Web Pages 1,0 (z wyjątkiem funkcji oznaczonych jako v2).</span><span class="sxs-lookup"><span data-stu-id="61246-110">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0 (except for features marked v2).</span></span>

<span data-ttu-id="61246-111">Ta strona zawiera informacje referencyjne dotyczące następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="61246-111">This page contains reference information for the following:</span></span>

- [<span data-ttu-id="61246-112">Klasy</span><span class="sxs-lookup"><span data-stu-id="61246-112">Classes</span></span>](#Classes)
- [<span data-ttu-id="61246-113">Dane</span><span class="sxs-lookup"><span data-stu-id="61246-113">Data</span></span>](#Data)
- [<span data-ttu-id="61246-114">Pomocnicy</span><span class="sxs-lookup"><span data-stu-id="61246-114">Helpers</span></span>](#Helpers)
- [<span data-ttu-id="61246-115">Walidacja</span><span class="sxs-lookup"><span data-stu-id="61246-115">Validation</span></span>](#Validation)

<a id="Classes"></a>
## <a name="classes"></a><span data-ttu-id="61246-116">Klasy</span><span class="sxs-lookup"><span data-stu-id="61246-116">Classes</span></span>

### `AppState[key], AppState[index],App`

<span data-ttu-id="61246-117">Zawiera dane, które mogą być współużytkowane przez wszystkie strony w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="61246-117">Contains data that can be shared by any pages in the application.</span></span> <span data-ttu-id="61246-118">Możesz użyć właściwości `App` dynamicznej, aby uzyskać dostęp do tych samych danych, jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="61246-118">You can use the dynamic `App` property to access the same data, as in the following example:</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample1.html)]

### `AsBool(), AsBool(true|false)`

<span data-ttu-id="61246-119">Konwertuje wartość ciągu na wartość logiczną (true/false).</span><span class="sxs-lookup"><span data-stu-id="61246-119">Converts a string value to a Boolean value (true/false).</span></span> <span data-ttu-id="61246-120">Zwraca wartość false lub wartość określającą, czy ciąg nie reprezentuje wartości true/false.</span><span class="sxs-lookup"><span data-stu-id="61246-120">Returns false or the specified value if the string does not represent true/false.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample2.cs)]

### `AsDateTime(), AsDateTime(value)`

<span data-ttu-id="61246-121">Konwertuje wartość ciągu na datę/godzinę.</span><span class="sxs-lookup"><span data-stu-id="61246-121">Converts a string value to date/time.</span></span> <span data-ttu-id="61246-122">Zwraca `DateTime.MinValue` lub określoną wartość, jeśli ciąg nie reprezentuje daty/godziny.</span><span class="sxs-lookup"><span data-stu-id="61246-122">Returns `DateTime.MinValue` or the specified value if the string does not represent a date/time.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample3.cs)]

### `AsDecimal(), AsDecimal(value)`

<span data-ttu-id="61246-123">Konwertuje wartość ciągu na wartość dziesiętną.</span><span class="sxs-lookup"><span data-stu-id="61246-123">Converts a string value to a decimal value.</span></span> <span data-ttu-id="61246-124">Zwraca 0,0 lub określoną wartość, jeśli ciąg nie reprezentuje wartości dziesiętnej.</span><span class="sxs-lookup"><span data-stu-id="61246-124">Returns 0.0 or the specified value if the string does not represent a decimal value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample4.cs)]

### `AsFloat(), AsFloat(value)`

<span data-ttu-id="61246-125">Konwertuje wartość ciągu na typ float.</span><span class="sxs-lookup"><span data-stu-id="61246-125">Converts a string value to a float.</span></span> <span data-ttu-id="61246-126">Zwraca 0,0 lub określoną wartość, jeśli ciąg nie reprezentuje wartości dziesiętnej.</span><span class="sxs-lookup"><span data-stu-id="61246-126">Returns 0.0 or the specified value if the string does not represent a decimal value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample5.cs)]

### `AsInt(), AsInt(value)`

<span data-ttu-id="61246-127">Konwertuje wartość ciągu na liczbę całkowitą.</span><span class="sxs-lookup"><span data-stu-id="61246-127">Converts a string value to an integer.</span></span> <span data-ttu-id="61246-128">Zwraca 0 lub określoną wartość, jeśli ciąg nie reprezentuje liczby całkowitej.</span><span class="sxs-lookup"><span data-stu-id="61246-128">Returns 0 or the specified value if the string does not represent an integer.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample6.cs)]

### `Href(path [, param1 [, param2]])`

<span data-ttu-id="61246-129">Tworzy adres URL zgodny z przeglądarką ze ścieżki pliku lokalnego z opcjonalnymi dodatkowymi częściami ścieżki.</span><span class="sxs-lookup"><span data-stu-id="61246-129">Creates a browser-compatible URL from a local file path, with optional additional path parts.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample7.cshtml)]

### `Html.Raw(value)`

<span data-ttu-id="61246-130">Renderuje *wartość* jako znacznik HTML, zamiast renderować ją jako dane wyjściowe kodowane w formacie HTML.</span><span class="sxs-lookup"><span data-stu-id="61246-130">Renders *value* as HTML markup instead of rendering it as HTML-encoded output.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample8.cshtml)]

### `IsBool(), IsDateTime(), IsDecimal(), IsFloat(), IsInt()`

<span data-ttu-id="61246-131">Zwraca wartość true, jeśli wartość może zostać przekonwertowana z ciągu na określony typ.</span><span class="sxs-lookup"><span data-stu-id="61246-131">Returns true if the value can be converted from a string to the specified type.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample9.cs)]

### `IsEmpty()`

<span data-ttu-id="61246-132">Zwraca wartość true, jeśli obiekt lub zmienna nie ma wartości.</span><span class="sxs-lookup"><span data-stu-id="61246-132">Returns true if the object or variable has no value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample10.cs)]

### `IsPost`

<span data-ttu-id="61246-133">Zwraca wartość PRAWDA, jeśli żądanie jest WPISem.</span><span class="sxs-lookup"><span data-stu-id="61246-133">Returns true if the request is a POST.</span></span> <span data-ttu-id="61246-134">(Żądania początkowe są zwykle GET).</span><span class="sxs-lookup"><span data-stu-id="61246-134">(Initial requests are usually a GET.)</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample11.cs)]

### `Layout`

<span data-ttu-id="61246-135">Określa ścieżkę strony układu, która ma zostać zastosowana do tej strony.</span><span class="sxs-lookup"><span data-stu-id="61246-135">Specifies the path of a layout page to apply to this page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample12.html)]

### `PageData[key], PageData[index],Page`

<span data-ttu-id="61246-136">Zawiera dane współdzielone między stroną, stronami układu i stronami częściowymi w bieżącym żądaniu.</span><span class="sxs-lookup"><span data-stu-id="61246-136">Contains data shared between the page, layout pages, and partial pages in the current request.</span></span> <span data-ttu-id="61246-137">Możesz użyć właściwości `Page` dynamicznej, aby uzyskać dostęp do tych samych danych, jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="61246-137">You can use the dynamic `Page` property to access the same data, as in the following example:</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample13.html)]

### `RenderBody()`

<span data-ttu-id="61246-138">(Strony układu) Renderuje zawartość strony zawartości, która nie znajduje się w nazwanych sekcjach.</span><span class="sxs-lookup"><span data-stu-id="61246-138">(Layout pages) Renders the content of a content page that is not in any named sections.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample14.cs)]

### `RenderPage(path, values)`  
`RenderPage(path[,param1 [, param2]])`

<span data-ttu-id="61246-139">Renderuje stronę zawartości przy użyciu określonej ścieżki i opcjonalnych dodatkowych danych.</span><span class="sxs-lookup"><span data-stu-id="61246-139">Renders a content page using the specified path and optional extra data.</span></span> <span data-ttu-id="61246-140">Można uzyskać wartości dodatkowych parametrów z `PageData` według pozycji (przykład 1) lub klucza (przykład 2).</span><span class="sxs-lookup"><span data-stu-id="61246-140">You can get the values of the extra parameters from `PageData` by position (example 1) or key (example 2).</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample15.js)]

### `RenderSection(sectionName [, required = true|false])`

<span data-ttu-id="61246-141">(Strony układu) Renderuje sekcję zawartości, która ma nazwę.</span><span class="sxs-lookup"><span data-stu-id="61246-141">(Layout pages) Renders a content section that has a name.</span></span> <span data-ttu-id="61246-142">Ustaw *wartość false, aby określić* , że sekcja jest opcjonalna.</span><span class="sxs-lookup"><span data-stu-id="61246-142">Set *required* to false to make a section optional.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample16.js)]

### `Request.Cookies[key]`

<span data-ttu-id="61246-143">Pobiera lub ustawia wartość pliku cookie HTTP.</span><span class="sxs-lookup"><span data-stu-id="61246-143">Gets or sets the value of an HTTP cookie.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample17.cs)]

### `Request.Files[key]`

<span data-ttu-id="61246-144">Pobiera pliki, które zostały przekazane w bieżącym żądaniu.</span><span class="sxs-lookup"><span data-stu-id="61246-144">Gets the files that were uploaded in the current request.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample18.js)]

### `Request.Form[key]`

<span data-ttu-id="61246-145">Pobiera dane, które zostały opublikowane w formularzu (jako ciągi).</span><span class="sxs-lookup"><span data-stu-id="61246-145">Gets data that was posted in a form (as strings).</span></span> <span data-ttu-id="61246-146">`Request[key]` sprawdza kolekcje `Request.Form` i `Request.QueryString`.</span><span class="sxs-lookup"><span data-stu-id="61246-146">`Request[key]` checks both the `Request.Form` and the `Request.QueryString` collections.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample19.cs)]

### `Request.QueryString[key]`

<span data-ttu-id="61246-147">Pobiera dane, które zostały określone w ciągu zapytania adresu URL.</span><span class="sxs-lookup"><span data-stu-id="61246-147">Gets data that was specified in the URL query string.</span></span> <span data-ttu-id="61246-148">`Request[key]` sprawdza kolekcje `Request.Form` i `Request.QueryString`.</span><span class="sxs-lookup"><span data-stu-id="61246-148">`Request[key]` checks both the `Request.Form` and the `Request.QueryString` collections.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample20.cs)]

### `Request.Unvalidated(key)`  
`Request.Unvalidated().QueryString|Form|Cookies|Headers[key]`

<span data-ttu-id="61246-149">Selektywnie wyłącza weryfikację żądań dla elementu formularza, wartości ciągu zapytania, pliku cookie lub wartości nagłówka.</span><span class="sxs-lookup"><span data-stu-id="61246-149">Selectively disables request validation for a form element, query-string value, cookie, or header value.</span></span> <span data-ttu-id="61246-150">Sprawdzanie poprawności żądań jest domyślnie włączone i uniemożliwia użytkownikom publikowanie znaczników lub innych potencjalnie niebezpiecznych treści.</span><span class="sxs-lookup"><span data-stu-id="61246-150">Request validation is enabled by default and prevents users from posting markup or other potentially dangerous content.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample21.cs)]

### `Response.AddHeader(name, value)`

<span data-ttu-id="61246-151">Dodaje do odpowiedzi nagłówek serwera HTTP.</span><span class="sxs-lookup"><span data-stu-id="61246-151">Adds an HTTP server header to the response.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample22.cs)]

### `Response.OutputCache(seconds [, sliding] [, varyByParams])`

<span data-ttu-id="61246-152">Buforuje dane wyjściowe strony przez określony czas.</span><span class="sxs-lookup"><span data-stu-id="61246-152">Caches the page output for a specified time.</span></span> <span data-ttu-id="61246-153">Opcjonalnie Ustaw *przesuwanie* w celu zresetowania limitu czasu dla każdej strony dostęp i *varyByParams* w celu buforowania różnych wersji strony dla każdego innego ciągu zapytania w żądaniu strony.</span><span class="sxs-lookup"><span data-stu-id="61246-153">Optionally set *sliding* to reset the timeout on each page access and *varyByParams* to cache different versions of the page for each different query string in the page request.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample23.js)]

### `Response.Redirect(path)`

<span data-ttu-id="61246-154">Przekierowuje żądanie przeglądarki do nowej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="61246-154">Redirects the browser request to a new location.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample24.js)]

### `Response.SetStatus(httpStatusCode)`

<span data-ttu-id="61246-155">Ustawia kod stanu HTTP wysyłany do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="61246-155">Sets the HTTP status code sent to the browser.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample25.cs)]

### `Response.WriteBinary(data [, mimetype])`

<span data-ttu-id="61246-156">Zapisuje zawartość *danych* do odpowiedzi przy użyciu opcjonalnego typu MIME.</span><span class="sxs-lookup"><span data-stu-id="61246-156">Writes the contents of *data* to the response with an optional MIME type.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample26.js)]

### `Response.WriteFile(file)`

<span data-ttu-id="61246-157">Zapisuje zawartość pliku do odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="61246-157">Writes the contents of a file to the response.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample27.cs)]

### `@section(sectionName) {content }`

<span data-ttu-id="61246-158">(Strony układu) Definiuje sekcję zawartości, która ma nazwę.</span><span class="sxs-lookup"><span data-stu-id="61246-158">(Layout pages) Defines a content section that has a name.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample28.cshtml)]

### `Server.HtmlDecode(htmlText)`

<span data-ttu-id="61246-159">Dekoduje ciąg, który jest zakodowany w formacie HTML.</span><span class="sxs-lookup"><span data-stu-id="61246-159">Decodes a string that is HTML encoded.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample29.cs)]

### `Server.HtmlEncode(text)`

<span data-ttu-id="61246-160">Koduje ciąg do renderowania w znaczniku HTML.</span><span class="sxs-lookup"><span data-stu-id="61246-160">Encodes a string for rendering in HTML markup.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample30.cs)]

### `Server.MapPath(virtualPath)`

<span data-ttu-id="61246-161">Zwraca ścieżkę fizyczną serwera dla określonej ścieżki wirtualnej.</span><span class="sxs-lookup"><span data-stu-id="61246-161">Returns the server physical path for the specified virtual path.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample31.cs)]

### `Server.UrlDecode(urlText)`

<span data-ttu-id="61246-162">Dekoduje tekst z adresu URL.</span><span class="sxs-lookup"><span data-stu-id="61246-162">Decodes text from a URL.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample32.cs)]

### `Server.UrlEncode(text)`

<span data-ttu-id="61246-163">Koduje tekst, który ma zostać umieszczony w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="61246-163">Encodes text to put in a URL.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample33.cs)]

### `Session[key]`

<span data-ttu-id="61246-164">Pobiera lub ustawia wartość, która istnieje, dopóki użytkownik nie zamknie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="61246-164">Gets or sets a value that exists until the user closes the browser.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample34.css)]

### `ToString()`

<span data-ttu-id="61246-165">Wyświetla reprezentację ciągu wartości obiektu.</span><span class="sxs-lookup"><span data-stu-id="61246-165">Displays a string representation of the object's value.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample35.html)]

### `UrlData[index]`

<span data-ttu-id="61246-166">Pobiera dodatkowe dane z adresu URL (na przykład */MyPage/ExtraData*).</span><span class="sxs-lookup"><span data-stu-id="61246-166">Gets additional data from the URL (for example, */MyPage/ExtraData*).</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample36.cs)]

### `WebSecurity.ChangePassword(userName,currentPassword,newPassword)`

<span data-ttu-id="61246-167">Zmienia hasło dla określonego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="61246-167">Changes the password for the specified user.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample37.cs)]

### `WebSecurity.ConfirmAccount(accountConfirmationToken)`

<span data-ttu-id="61246-168">Potwierdza konto przy użyciu tokenu potwierdzenia konta.</span><span class="sxs-lookup"><span data-stu-id="61246-168">Confirms an account using the account confirmation token.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample38.cs)]

### `WebSecurity.CreateAccount(userName, password`  
 `[, requireConfirmationToken = true|false])`

<span data-ttu-id="61246-169">Tworzy nowe konto użytkownika z określoną nazwą użytkownika i hasłem.</span><span class="sxs-lookup"><span data-stu-id="61246-169">Creates a new user account with the specified user name and password.</span></span> <span data-ttu-id="61246-170">Aby wymagać tokenu potwierdzenia, należy przekazać wartość true dla *requireConfirmationToken.*</span><span class="sxs-lookup"><span data-stu-id="61246-170">To require a confirmation token, pass true for *requireConfirmationToken.*</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample39.cs)]

### `WebSecurity.CurrentUserId`

<span data-ttu-id="61246-171">Pobiera identyfikator liczby całkowitej dla aktualnie zalogowanego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="61246-171">Gets the integer identifier for the currently logged-in user.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample40.cs)]

### `WebSecurity.CurrentUserName`

<span data-ttu-id="61246-172">Pobiera nazwę aktualnie zalogowanego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="61246-172">Gets the name for the currently logged-in user.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample41.cs)]

### `WebSecurity.GeneratePasswordResetToken(username`  
 `[, tokenExpirationInMinutesFromNow])`

<span data-ttu-id="61246-173">Generuje token resetowania hasła, który można wysłać w wiadomości e-mail do użytkownika, aby użytkownik mógł zresetować hasło.</span><span class="sxs-lookup"><span data-stu-id="61246-173">Generates a password-reset token that can be sent in email to a user so that the user can reset the password.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample42.cs)]

### `WebSecurity.GetUserId(userName)`

<span data-ttu-id="61246-174">Zwraca identyfikator użytkownika z nazwy użytkownika.</span><span class="sxs-lookup"><span data-stu-id="61246-174">Returns the user ID from the user name.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample43.cs)]

### `WebSecurity.IsAuthenticated`

<span data-ttu-id="61246-175">Zwraca wartość PRAWDA, jeśli bieżący użytkownik jest zalogowany.</span><span class="sxs-lookup"><span data-stu-id="61246-175">Returns true if the current user is logged in.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample44.cs)]

### `WebSecurity.IsConfirmed(userName)`

<span data-ttu-id="61246-176">Zwraca wartość PRAWDA, jeśli użytkownik został potwierdzony (na przykład za pośrednictwem wiadomości e-mail z potwierdzeniem).</span><span class="sxs-lookup"><span data-stu-id="61246-176">Returns true if the user has been confirmed (for example, through a confirmation email).</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample45.cs)]

### `WebSecurity.IsCurrentUser(userName)`

<span data-ttu-id="61246-177">Zwraca wartość PRAWDA, jeśli nazwa bieżącego użytkownika jest zgodna z określoną nazwą użytkownika.</span><span class="sxs-lookup"><span data-stu-id="61246-177">Returns true if the current user's name matches the specified user name.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample46.cs)]

### `WebSecurity.Login(userName,password[, persistCookie])`

<span data-ttu-id="61246-178">Rejestruje użytkownika w programie, ustawiając token uwierzytelniania w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="61246-178">Logs the user in by setting an authentication token in the cookie.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample47.cs)]

### `WebSecurity.Logout()`

<span data-ttu-id="61246-179">Wylogowuje użytkownika przez usunięcie pliku cookie tokenu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="61246-179">Logs the user out by removing the authentication token cookie.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample48.css)]

### `WebSecurity.RequireAuthenticatedUser()`

<span data-ttu-id="61246-180">Jeśli użytkownik nie jest uwierzytelniony, ustawia stan HTTP na wartość 401 (Bez autoryzacji).</span><span class="sxs-lookup"><span data-stu-id="61246-180">If the user is not authenticated, sets the HTTP status to 401 (Unauthorized).</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample49.css)]

### `WebSecurity.RequireRoles(roles)`

<span data-ttu-id="61246-181">Jeśli bieżący użytkownik nie jest członkiem jednej z określonych ról, ustawia stan HTTP na 401 (nieautoryzowany).</span><span class="sxs-lookup"><span data-stu-id="61246-181">If the current user is not a member of one of the specified roles, sets the HTTP status to 401 (Unauthorized).</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample50.html)]

### `WebSecurity.RequireUser(userId)`  
`WebSecurity.RequireUser(userName)`

<span data-ttu-id="61246-182">Jeśli bieżący użytkownik nie jest użytkownikiem *określonym przez użytkownika*, ustawia stan HTTP na 401 (nieautoryzowany).</span><span class="sxs-lookup"><span data-stu-id="61246-182">If the current user is not the user specified by *username*, sets the HTTP status to 401 (Unauthorized).</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample51.css)]

### `WebSecurity.ResetPassword(passwordResetToken,newPassword)`

<span data-ttu-id="61246-183">Jeśli token resetowania hasła jest prawidłowy, program zmieni hasło użytkownika na nowe hasło.</span><span class="sxs-lookup"><span data-stu-id="61246-183">If the password reset token is valid, changes the user's password to the new password.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample52.css)]

<a id="Data"></a>
## <a name="data"></a><span data-ttu-id="61246-184">Dane</span><span class="sxs-lookup"><span data-stu-id="61246-184">Data</span></span>

### `Database.Execute(SQLstatement [,parameters]`

<span data-ttu-id="61246-185">Wykonuje element *sqlstateing* (z opcjonalnymi parametrami), taki jak INSERT, DELETE lub Update i zwraca liczbę rekordów, których to dotyczy.</span><span class="sxs-lookup"><span data-stu-id="61246-185">Executes *SQLstatement* (with optional parameters) such as INSERT, DELETE, or UPDATE and returns a count of affected records.</span></span>

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample53.sql)]

### `Database.GetLastInsertId()`

<span data-ttu-id="61246-186">Zwraca kolumnę tożsamości z ostatnio wstawionego wiersza.</span><span class="sxs-lookup"><span data-stu-id="61246-186">Returns the identity column from the most recently inserted row.</span></span>

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample54.sql)]

### `Database.Open(filename)`  
`Database.Open(connectionStringName)`

<span data-ttu-id="61246-187">Otwiera określony plik bazy danych lub bazę danych określoną przy użyciu nazwanych parametrów połączenia z pliku *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="61246-187">Opens either the specified database file or the database specified using a named connection string from the *Web.config* file.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample55.cs)]

### `Database.OpenConnectionString(connectionString)`

<span data-ttu-id="61246-188">Otwiera bazę danych przy użyciu parametrów połączenia.</span><span class="sxs-lookup"><span data-stu-id="61246-188">Opens a database using the connection string.</span></span> <span data-ttu-id="61246-189">(Ten kontrast `Database.Open`, który używa nazwy parametrów połączenia).</span><span class="sxs-lookup"><span data-stu-id="61246-189">(This contrasts with `Database.Open`, which uses a connection string name.)</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample56.cs)]

### `Database.Query(SQLstatement[,parameters])`

<span data-ttu-id="61246-190">Wysyła zapytanie do bazy danych przy użyciu *sqlstateion* (opcjonalnie przekazując parametry) i zwraca wyniki jako kolekcję.</span><span class="sxs-lookup"><span data-stu-id="61246-190">Queries the database using *SQLstatement* (optionally passing parameters) and returns the results as a collection.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample57.html)]

### `Database.QuerySingle(SQLstatement [, parameters])`

<span data-ttu-id="61246-191">Wykonuje element *sqlstateing* (z opcjonalnymi parametrami) i zwraca jeden rekord.</span><span class="sxs-lookup"><span data-stu-id="61246-191">Executes *SQLstatement* (with optional parameters) and returns a single record.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample58.cs)]

### `Database.QueryValue(SQLstatement [, parameters])`

<span data-ttu-id="61246-192">Wykonuje element *sqlstateing* (z opcjonalnymi parametrami) i zwraca pojedynczą wartość.</span><span class="sxs-lookup"><span data-stu-id="61246-192">Executes *SQLstatement* (with optional parameters) and returns a single value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample59.cs)]

<a id="Helpers"></a>
## <a name="helpers"></a><span data-ttu-id="61246-193">Pomocnicy</span><span class="sxs-lookup"><span data-stu-id="61246-193">Helpers</span></span>

### `Analytics.GetGoogleHtml(webPropertyId)`

<span data-ttu-id="61246-194">Renderuje kod JavaScript usługi Google Analytics dla podanego identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="61246-194">Renders the Google Analytics JavaScript code for the specified ID.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample60.js)]

### `Analytics.GetStatCounterHtml(project,security)`

<span data-ttu-id="61246-195">Renderuje kod JavaScript StatCounter Analytics dla określonego projektu.</span><span class="sxs-lookup"><span data-stu-id="61246-195">Renders the StatCounter Analytics JavaScript code for the specified project.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample61.css)]

### `Analytics.GetYahooHtml(account)`

<span data-ttu-id="61246-196">Renderuje kod JavaScript usługi Yahoo Analytics dla określonego konta.</span><span class="sxs-lookup"><span data-stu-id="61246-196">Renders the Yahoo Analytics JavaScript code for the specified account.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample62.js)]

### `Bing.SearchBox([boxWidth])`

<span data-ttu-id="61246-197">Przekazuje wyszukiwanie do usługi Bing.</span><span class="sxs-lookup"><span data-stu-id="61246-197">Passes a search to Bing.</span></span> <span data-ttu-id="61246-198">Aby określić lokację do przeszukania i tytuł pola wyszukiwania, można ustawić właściwości `Bing.SiteUrl` i `Bing.SiteTitle`.</span><span class="sxs-lookup"><span data-stu-id="61246-198">To specify the site to search and a title for the search box, you can set the `Bing.SiteUrl` and `Bing.SiteTitle` properties.</span></span> <span data-ttu-id="61246-199">Zwykle te właściwości są ustawiane na stronie *\_AppStart* .</span><span class="sxs-lookup"><span data-stu-id="61246-199">Normally you set these properties in the *\_AppStart* page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample63.html)]

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample64.cshtml)]

### `Chart(width,height [, template] [, templatePath])`

<span data-ttu-id="61246-200">Inicjuje wykres.</span><span class="sxs-lookup"><span data-stu-id="61246-200">Initializes a chart.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample65.cshtml)]

### `Chart.AddLegend([title] [, name])`

<span data-ttu-id="61246-201">Dodaje legendę do wykresu.</span><span class="sxs-lookup"><span data-stu-id="61246-201">Adds a legend to a chart.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample66.cshtml)]

### `Chart.AddSeries([name] [, chartType] [, chartArea]`  
 `[, axisLabel] [, legend] [, markerStep] [, xValue]`  
 `[, xField] [, yValues] [, yFields] [, options])`

<span data-ttu-id="61246-202">Dodaje serię wartości do wykresu.</span><span class="sxs-lookup"><span data-stu-id="61246-202">Adds a series of values to the chart.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample67.cshtml)]

### `Crypto.Hash(string [, algorithm])`  
`Crypto.Hash(bytes [, algorithm])`

<span data-ttu-id="61246-203">Zwraca wartość skrótu dla określonych danych.</span><span class="sxs-lookup"><span data-stu-id="61246-203">Returns a hash for the specified data.</span></span> <span data-ttu-id="61246-204">Domyślny algorytm to `sha256`.</span><span class="sxs-lookup"><span data-stu-id="61246-204">The default algorithm is `sha256`.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample68.html)]

### `Facebook.LikeButton(href [, buttonLayout] [, showFaces] [, width] [, height]`   
 `[, action] [, font] [, colorScheme] [, refLabel])`

<span data-ttu-id="61246-205">Umożliwia użytkownikom serwisu Facebook nawiązywanie połączenia z stronami.</span><span class="sxs-lookup"><span data-stu-id="61246-205">Lets Facebook users make a connection to pages.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample69.js)]

### `FileUpload.GetHtml([initialNumberOfFiles] [, allowMoreFilesToBeAdded]`  
 `[, includeFormTag] [, addText] [, uploadText])`

<span data-ttu-id="61246-206">Renderuje interfejs użytkownika do przekazywania plików.</span><span class="sxs-lookup"><span data-stu-id="61246-206">Renders UI for uploading files.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample70.html)]

### `GamerCard.GetHtml(gamerTag)`

<span data-ttu-id="61246-207">Renderuje określony tag usługi Xbox graczy.</span><span class="sxs-lookup"><span data-stu-id="61246-207">Renders the specified Xbox gamer tag.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample71.js)]

### `Gravatar.GetHtml(email [, imageSize] [, defaultImage] [, rating]`  
 `[, imageExtension] [, attributes])`

<span data-ttu-id="61246-208">Renderuje obraz skonfigurowali Gravatar dla podanego adresu e-mail.</span><span class="sxs-lookup"><span data-stu-id="61246-208">Renders the Gravatar image for the specified email address.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample72.css)]

### `Json.Encode(object)`

<span data-ttu-id="61246-209">Konwertuje obiekt danych na ciąg w formacie JavaScript Object Notation (JSON).</span><span class="sxs-lookup"><span data-stu-id="61246-209">Converts a data object to a string in the JavaScript Object Notation (JSON) format.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample73.cs)]

### `Json.Decode(string)`

<span data-ttu-id="61246-210">Konwertuje ciąg wejściowy zakodowany w formacie JSON na obiekt danych, który można wykonać iterację lub wstawić do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="61246-210">Converts a JSON-encoded input string to a data object that you can iterate over or insert into a database.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample74.cs)]

### `LinkShare.GetHtml(pageTitle[, pageLinkBack] [, twitterUserName]`  
 `[, additionalTweetText] [, linkSites])`

<span data-ttu-id="61246-211">Renderuje linki do sieci społecznościowych przy użyciu określonego tytułu i opcjonalnego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="61246-211">Renders social networking links using the specified title and optional URL.</span></span>

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample75.xml)]

### `ModelStateDictionary.AddError(key, errorMessage)`

<span data-ttu-id="61246-212">Kojarzy komunikat o błędzie z polem formularza.</span><span class="sxs-lookup"><span data-stu-id="61246-212">Associates an error message with a form field.</span></span> <span data-ttu-id="61246-213">Użyj pomocnika `ModelState`, aby uzyskać dostęp do tego elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="61246-213">Use the `ModelState` helper to access this member.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample76.cs)]

### `ModelStateDictionary.AddFormError(errorMessage)`

<span data-ttu-id="61246-214">Kojarzy komunikat o błędzie z formularzem.</span><span class="sxs-lookup"><span data-stu-id="61246-214">Associates an error message with a form.</span></span> <span data-ttu-id="61246-215">Użyj pomocnika `ModelState`, aby uzyskać dostęp do tego elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="61246-215">Use the `ModelState` helper to access this member.</span></span>

[!code-powershell[Main](asp-net-web-pages-api-reference/samples/sample77.ps1)]

### `ModelStateDictionary.IsValid`

<span data-ttu-id="61246-216">Zwraca wartość true, jeśli nie występują błędy walidacji.</span><span class="sxs-lookup"><span data-stu-id="61246-216">Returns true if there are no validation errors.</span></span> <span data-ttu-id="61246-217">Użyj pomocnika `ModelState`, aby uzyskać dostęp do tego elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="61246-217">Use the `ModelState` helper to access this member.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample78.cs)]

### `ObjectInfo.Print(value [, depth] [, enumerationLength])`

<span data-ttu-id="61246-218">Renderuje właściwości i wartości obiektu i wszystkich obiektów podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="61246-218">Renders the properties and values of an object and any child objects.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample79.css)]

### `Recaptcha.GetHtml([, publicKey] [, theme] [, language] [, tabIndex])`

<span data-ttu-id="61246-219">Renderuje test weryfikacyjny reCAPTCHA.</span><span class="sxs-lookup"><span data-stu-id="61246-219">Renders the reCAPTCHA verification test.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample80.css)]

### `ReCaptcha.PublicKey`  
 `ReCaptcha.PrivateKey`

<span data-ttu-id="61246-220">Ustawia klucze publiczne i prywatne dla usługi reCAPTCHA.</span><span class="sxs-lookup"><span data-stu-id="61246-220">Sets public and private keys for the reCAPTCHA service.</span></span> <span data-ttu-id="61246-221">Zwykle te właściwości są ustawiane na stronie *\_AppStart* .</span><span class="sxs-lookup"><span data-stu-id="61246-221">Normally you set these properties in the *\_AppStart* page.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample81.css)]

### `ReCaptcha.Validate([, privateKey])`

<span data-ttu-id="61246-222">Zwraca wynik testu reCAPTCHA.</span><span class="sxs-lookup"><span data-stu-id="61246-222">Returns the result of the reCAPTCHA test.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample82.cs)]

### `ServerInfo.GetHtml()`

<span data-ttu-id="61246-223">Renderuje informacje o stanie dotyczące stron sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="61246-223">Renders status information about ASP.NET Web Pages.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample83.cshtml)]

### `Twitter.Profile(twitterUserName)`

<span data-ttu-id="61246-224">Renderuje strumień usługi Twitter dla określonego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="61246-224">Renders a Twitter stream for the specified user.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample84.js)]

### `Twitter.Search(searchQuery)`

<span data-ttu-id="61246-225">Renderuje strumień usługi Twitter dla określonego tekstu wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="61246-225">Renders a Twitter stream for the specified search text.</span></span>

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample85.xml)]

### `Video.Flash(filename [, width, height])`

<span data-ttu-id="61246-226">Renderuje odtwarzacz wideo Flash dla określonego pliku z opcjonalną szerokością i wysokością.</span><span class="sxs-lookup"><span data-stu-id="61246-226">Renders a Flash video player for the specified file with optional width and height.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample86.cshtml)]

### `Video.MediaPlayer(filename [, width, height])`

<span data-ttu-id="61246-227">Renderuje program Windows Media Player dla określonego pliku z opcjonalną szerokością i wysokością.</span><span class="sxs-lookup"><span data-stu-id="61246-227">Renders a Windows Media player for the specified file with optional width and height.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample87.cshtml)]

### `Video.Silverlight(filename, width, height)`

<span data-ttu-id="61246-228">Renderuje odtwarzacz Silverlight dla określonego pliku *XAP* z wymaganą szerokością i wysokością.</span><span class="sxs-lookup"><span data-stu-id="61246-228">Renders a Silverlight player for the specified *.xap* file with required width and height.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample88.cshtml)]

### `WebCache.Get(key)`

<span data-ttu-id="61246-229">Zwraca obiekt określony przez *klucz*lub wartość null, jeśli nie można odnaleźć obiektu.</span><span class="sxs-lookup"><span data-stu-id="61246-229">Returns the object specified by *key*, or null if the object is not found.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample89.cs)]

### `WebCache.Remove(key)`

<span data-ttu-id="61246-230">Usuwa obiekt określony przez *klucz* z pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="61246-230">Removes the object specified by *key* from the cache.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample90.cs)]

### `WebCache.Set(key, value [, minutesToCache] [, slidingExpiration])`

<span data-ttu-id="61246-231">Umieszcza *wartość* w pamięci podręcznej pod nazwą określoną przez *klucz*.</span><span class="sxs-lookup"><span data-stu-id="61246-231">Puts *value* into the cache under the name specified by *key*.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample91.html)]

### `WebGrid(data)`

<span data-ttu-id="61246-232">Tworzy nowy obiekt `WebGrid` przy użyciu danych z zapytania.</span><span class="sxs-lookup"><span data-stu-id="61246-232">Creates a new `WebGrid` object using data from a query.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample92.cs)]

### `WebGrid.GetHtml()`

<span data-ttu-id="61246-233">Renderuje znaczniki, aby wyświetlić dane w tabeli HTML.</span><span class="sxs-lookup"><span data-stu-id="61246-233">Renders markup to display data in an HTML table.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample93.html)]

### `WebGrid.Pager()`

<span data-ttu-id="61246-234">Renderuje moduł stronicowania dla obiektu `WebGrid`.</span><span class="sxs-lookup"><span data-stu-id="61246-234">Renders a pager for the `WebGrid` object.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample94.html)]

### `WebImage(path)`

<span data-ttu-id="61246-235">Ładuje obraz z określonej ścieżki.</span><span class="sxs-lookup"><span data-stu-id="61246-235">Loads an image from the specified path.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample95.cs)]

### `WebImage.AddImagesWatermark(image)`

<span data-ttu-id="61246-236">Dodaje określony obraz jako znak wodny.</span><span class="sxs-lookup"><span data-stu-id="61246-236">Adds the specified image as a watermark.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample96.cs)]

### `WebImage.AddTextWatermark(text)`

<span data-ttu-id="61246-237">Dodaje określony tekst do obrazu.</span><span class="sxs-lookup"><span data-stu-id="61246-237">Adds the specified text to the image.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample97.cs)]

### `WebImage.FlipHorizontal()`  
`WebImage.FlipVertical()`

<span data-ttu-id="61246-238">Przerzuca obraz w poziomie lub w pionie.</span><span class="sxs-lookup"><span data-stu-id="61246-238">Flips the image horizontally or vertically.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample98.css)]

### `WebImage.GetImageFromRequest()`

<span data-ttu-id="61246-239">Ładuje obraz, gdy obraz zostanie opublikowany na stronie podczas przekazywania pliku.</span><span class="sxs-lookup"><span data-stu-id="61246-239">Loads an image when an image is posted to a page during a file upload.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample99.cs)]

### `WebImage.Resize(width,height)`

<span data-ttu-id="61246-240">Zmienia rozmiar obrazu.</span><span class="sxs-lookup"><span data-stu-id="61246-240">Resizes an the image.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample100.css)]

### `WebImage.RotateLeft()`  
`WebImage.RotateRight()`

<span data-ttu-id="61246-241">Obraca obraz w lewo lub w prawo.</span><span class="sxs-lookup"><span data-stu-id="61246-241">Rotates the image to the left or the right.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample101.css)]

### `WebImage.Save(path [, imageFormat])`

<span data-ttu-id="61246-242">Zapisuje obraz do określonej ścieżki.</span><span class="sxs-lookup"><span data-stu-id="61246-242">Saves the image to the specified path.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample102.js)]

### `WebMail.Password`

<span data-ttu-id="61246-243">Ustawia hasło dla serwera SMTP.</span><span class="sxs-lookup"><span data-stu-id="61246-243">Sets the password for the SMTP server.</span></span> <span data-ttu-id="61246-244">Zwykle tę właściwość można ustawić na stronie *\_AppStart* .</span><span class="sxs-lookup"><span data-stu-id="61246-244">Normally you set this property in the *\_AppStart* page.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample103.cs)]

### `WebMail.Send(to, subject, body [, from] [, cc] [, filesToAttach] [, isBodyHtml]`  
 `[, additionalHeaders])`

<span data-ttu-id="61246-245">Wysyła wiadomość e-mail.</span><span class="sxs-lookup"><span data-stu-id="61246-245">Sends an email message.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample104.css)]

### `WebMail.SmtpServer`

<span data-ttu-id="61246-246">Ustawia nazwę serwera SMTP.</span><span class="sxs-lookup"><span data-stu-id="61246-246">Sets the SMTP server name.</span></span> <span data-ttu-id="61246-247">Zwykle tę właściwość można ustawić na stronie *\_AppStart* .</span><span class="sxs-lookup"><span data-stu-id="61246-247">Normally you set this property in the *\_AppStart* page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample105.html)]

### `WebMail.UserName`

<span data-ttu-id="61246-248">Ustawia nazwę użytkownika dla serwera SMTP.</span><span class="sxs-lookup"><span data-stu-id="61246-248">Sets the user name for the SMTP server.</span></span> <span data-ttu-id="61246-249">Zwykle należy ustawić tę właściwość na stronie *\_AppStart* .</span><span class="sxs-lookup"><span data-stu-id="61246-249">Normally you should set this property in the *\_AppStart* page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample106.html)]

<a id="Validation"></a>
## <a name="validation"></a><span data-ttu-id="61246-250">Walidacja</span><span class="sxs-lookup"><span data-stu-id="61246-250">Validation</span></span>

### `Html.ValidationMessage(field)`

<span data-ttu-id="61246-251">v Renderuje komunikat o błędzie walidacji dla określonego pola.</span><span class="sxs-lookup"><span data-stu-id="61246-251">(v2) Renders a validation error message for the specified field.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample107.cshtml)]

### `Html.ValidationSummary([message])`

<span data-ttu-id="61246-252">v Wyświetla listę wszystkich błędów walidacji.</span><span class="sxs-lookup"><span data-stu-id="61246-252">(v2) Displays a list of all validation errors.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample108.cshtml)]

### `Validation.Add(field, validationType)`

<span data-ttu-id="61246-253">v Rejestruje element danych wejściowych użytkownika dla określonego typu walidacji.</span><span class="sxs-lookup"><span data-stu-id="61246-253">(v2) Registers a user input element for the specified type of validation.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample109.js)]

### `Validation.ClassFor(field)`

<span data-ttu-id="61246-254">v Dynamicznie renderuje atrybuty klasy CSS dla weryfikacji po stronie klienta, aby można było sformatować komunikaty o błędach walidacji.</span><span class="sxs-lookup"><span data-stu-id="61246-254">(v2) Dynamically renders CSS class attributes for client-side validation so that you can format validation error messages.</span></span> <span data-ttu-id="61246-255">(Wymaga odwołania do odpowiednich bibliotek skryptu klienta i zdefiniowania klas CSS).</span><span class="sxs-lookup"><span data-stu-id="61246-255">(Requires that you reference the appropriate client-script libraries and that you define CSS classes.)</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample110.html)]

### `Validation.For(field)`

<span data-ttu-id="61246-256">v Włącza weryfikację po stronie klienta dla pola danych wejściowych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="61246-256">(v2) Enables client-side validation for the user input field.</span></span> <span data-ttu-id="61246-257">(Wymaga odwołania do odpowiednich bibliotek skryptu klienta).</span><span class="sxs-lookup"><span data-stu-id="61246-257">(Requires that you reference the appropriate client-script libraries.)</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample111.html)]

### `Validation.IsValid()`

<span data-ttu-id="61246-258">v Zwraca wartość PRAWDA, jeśli wszystkie elementy danych wejściowych użytkownika, które są registred do walidacji, zawierają prawidłowe wartości.</span><span class="sxs-lookup"><span data-stu-id="61246-258">(v2) Returns true if all user input elements that are registred for validation contain valid values.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample112.cs)]

### `Validation.RequireField(field[, errorMessage])`

<span data-ttu-id="61246-259">v Określa, że użytkownicy muszą podać wartość dla elementu danych wejściowych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="61246-259">(v2) Specifies that users must provide a value for the user input element.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample113.cs)]

### `Validation.RequireFields(field1[, field12, field3, ...])`

<span data-ttu-id="61246-260">v Określa, że użytkownicy muszą podać wartości dla każdego elementu danych wejściowych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="61246-260">(v2) Specifies that users must provide values for each of the user input elements.</span></span> <span data-ttu-id="61246-261">Ta metoda nie pozwala na określenie niestandardowego komunikatu o błędzie.</span><span class="sxs-lookup"><span data-stu-id="61246-261">This method does not let you specify a custom error message.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample114.html)]

### `Validator.DateTime ([error message])`  
`Validator.Decimal([error message])`  
`Validator.EqualsTo(otherField,[error message])`  
`Validator.Float([error message])`  
`Validator.Integer([error message])`  
`Validator.Range(min,max [, error message])`  
`Validator.RegEx(pattern[, error message])`  
`Validator.Required([error message])`  
`Validator.StringLength(length)`  
`Validator.Url([error message])`

<span data-ttu-id="61246-262">v Określa test weryfikacyjny przy użyciu metody `Validation.Add`.</span><span class="sxs-lookup"><span data-stu-id="61246-262">(v2) Specifies a validation test when you use the `Validation.Add` method.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample115.js)]
