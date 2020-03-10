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
# <a name="aspnet-web-pages-razor-api-quick-reference"></a>Krótkie informacje o interfejsie API ASP.NET Web Pages (Razor)

Autor [FitzMacken](https://github.com/tfitzmac)

> Ta strona zawiera listę z krótkimi przykładami najczęściej używanych obiektów, właściwościami i metodami programowania ASP.NET stron sieci Web za pomocą składnia Razor.
> 
> Opisy oznaczone jako "(v2)" zostały wprowadzone w witrynie sieci Web ASP.NET w wersji 2.
> 
> Aby uzyskać dokumentację referencyjną interfejsu API, zobacz dokumentację dotyczącą [stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=208659) w witrynie MSDN.
> 
> ## <a name="software-versions"></a>Wersje oprogramowania
> 
> 
> - ASP.NET strony sieci Web (Razor) 3
>   
> 
> Ten samouczek działa również z ASP.NET Web Pages 2 i ASP.NET Web Pages 1,0 (z wyjątkiem funkcji oznaczonych jako v2).

Ta strona zawiera informacje referencyjne dotyczące następujących czynności:

- [Klasy](#Classes)
- [Dane](#Data)
- [Pomocnicy](#Helpers)
- [Walidacja](#Validation)

<a id="Classes"></a>
## <a name="classes"></a>Klasy

### `AppState[key], AppState[index],App`

Zawiera dane, które mogą być współużytkowane przez wszystkie strony w aplikacji. Możesz użyć właściwości `App` dynamicznej, aby uzyskać dostęp do tych samych danych, jak w poniższym przykładzie:

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample1.html)]

### `AsBool(), AsBool(true|false)`

Konwertuje wartość ciągu na wartość logiczną (true/false). Zwraca wartość false lub wartość określającą, czy ciąg nie reprezentuje wartości true/false.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample2.cs)]

### `AsDateTime(), AsDateTime(value)`

Konwertuje wartość ciągu na datę/godzinę. Zwraca `DateTime.MinValue` lub określoną wartość, jeśli ciąg nie reprezentuje daty/godziny.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample3.cs)]

### `AsDecimal(), AsDecimal(value)`

Konwertuje wartość ciągu na wartość dziesiętną. Zwraca 0,0 lub określoną wartość, jeśli ciąg nie reprezentuje wartości dziesiętnej.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample4.cs)]

### `AsFloat(), AsFloat(value)`

Konwertuje wartość ciągu na typ float. Zwraca 0,0 lub określoną wartość, jeśli ciąg nie reprezentuje wartości dziesiętnej.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample5.cs)]

### `AsInt(), AsInt(value)`

Konwertuje wartość ciągu na liczbę całkowitą. Zwraca 0 lub określoną wartość, jeśli ciąg nie reprezentuje liczby całkowitej.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample6.cs)]

### `Href(path [, param1 [, param2]])`

Tworzy adres URL zgodny z przeglądarką ze ścieżki pliku lokalnego z opcjonalnymi dodatkowymi częściami ścieżki.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample7.cshtml)]

### `Html.Raw(value)`

Renderuje *wartość* jako znacznik HTML, zamiast renderować ją jako dane wyjściowe kodowane w formacie HTML.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample8.cshtml)]

### `IsBool(), IsDateTime(), IsDecimal(), IsFloat(), IsInt()`

Zwraca wartość true, jeśli wartość może zostać przekonwertowana z ciągu na określony typ.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample9.cs)]

### `IsEmpty()`

Zwraca wartość true, jeśli obiekt lub zmienna nie ma wartości.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample10.cs)]

### `IsPost`

Zwraca wartość PRAWDA, jeśli żądanie jest WPISem. (Żądania początkowe są zwykle GET).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample11.cs)]

### `Layout`

Określa ścieżkę strony układu, która ma zostać zastosowana do tej strony.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample12.html)]

### `PageData[key], PageData[index],Page`

Zawiera dane współdzielone między stroną, stronami układu i stronami częściowymi w bieżącym żądaniu. Możesz użyć właściwości `Page` dynamicznej, aby uzyskać dostęp do tych samych danych, jak w poniższym przykładzie:

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample13.html)]

### `RenderBody()`

(Strony układu) Renderuje zawartość strony zawartości, która nie znajduje się w nazwanych sekcjach.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample14.cs)]

### `RenderPage(path, values)`  
`RenderPage(path[,param1 [, param2]])`

Renderuje stronę zawartości przy użyciu określonej ścieżki i opcjonalnych dodatkowych danych. Można uzyskać wartości dodatkowych parametrów z `PageData` według pozycji (przykład 1) lub klucza (przykład 2).

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample15.js)]

### `RenderSection(sectionName [, required = true|false])`

(Strony układu) Renderuje sekcję zawartości, która ma nazwę. Ustaw *wartość false, aby określić* , że sekcja jest opcjonalna.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample16.js)]

### `Request.Cookies[key]`

Pobiera lub ustawia wartość pliku cookie HTTP.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample17.cs)]

### `Request.Files[key]`

Pobiera pliki, które zostały przekazane w bieżącym żądaniu.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample18.js)]

### `Request.Form[key]`

Pobiera dane, które zostały opublikowane w formularzu (jako ciągi). `Request[key]` sprawdza kolekcje `Request.Form` i `Request.QueryString`.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample19.cs)]

### `Request.QueryString[key]`

Pobiera dane, które zostały określone w ciągu zapytania adresu URL. `Request[key]` sprawdza kolekcje `Request.Form` i `Request.QueryString`.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample20.cs)]

### `Request.Unvalidated(key)`  
`Request.Unvalidated().QueryString|Form|Cookies|Headers[key]`

Selektywnie wyłącza weryfikację żądań dla elementu formularza, wartości ciągu zapytania, pliku cookie lub wartości nagłówka. Sprawdzanie poprawności żądań jest domyślnie włączone i uniemożliwia użytkownikom publikowanie znaczników lub innych potencjalnie niebezpiecznych treści.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample21.cs)]

### `Response.AddHeader(name, value)`

Dodaje do odpowiedzi nagłówek serwera HTTP.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample22.cs)]

### `Response.OutputCache(seconds [, sliding] [, varyByParams])`

Buforuje dane wyjściowe strony przez określony czas. Opcjonalnie Ustaw *przesuwanie* w celu zresetowania limitu czasu dla każdej strony dostęp i *varyByParams* w celu buforowania różnych wersji strony dla każdego innego ciągu zapytania w żądaniu strony.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample23.js)]

### `Response.Redirect(path)`

Przekierowuje żądanie przeglądarki do nowej lokalizacji.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample24.js)]

### `Response.SetStatus(httpStatusCode)`

Ustawia kod stanu HTTP wysyłany do przeglądarki.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample25.cs)]

### `Response.WriteBinary(data [, mimetype])`

Zapisuje zawartość *danych* do odpowiedzi przy użyciu opcjonalnego typu MIME.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample26.js)]

### `Response.WriteFile(file)`

Zapisuje zawartość pliku do odpowiedzi.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample27.cs)]

### `@section(sectionName) {content }`

(Strony układu) Definiuje sekcję zawartości, która ma nazwę.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample28.cshtml)]

### `Server.HtmlDecode(htmlText)`

Dekoduje ciąg, który jest zakodowany w formacie HTML.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample29.cs)]

### `Server.HtmlEncode(text)`

Koduje ciąg do renderowania w znaczniku HTML.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample30.cs)]

### `Server.MapPath(virtualPath)`

Zwraca ścieżkę fizyczną serwera dla określonej ścieżki wirtualnej.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample31.cs)]

### `Server.UrlDecode(urlText)`

Dekoduje tekst z adresu URL.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample32.cs)]

### `Server.UrlEncode(text)`

Koduje tekst, który ma zostać umieszczony w adresie URL.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample33.cs)]

### `Session[key]`

Pobiera lub ustawia wartość, która istnieje, dopóki użytkownik nie zamknie przeglądarki.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample34.css)]

### `ToString()`

Wyświetla reprezentację ciągu wartości obiektu.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample35.html)]

### `UrlData[index]`

Pobiera dodatkowe dane z adresu URL (na przykład */MyPage/ExtraData*).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample36.cs)]

### `WebSecurity.ChangePassword(userName,currentPassword,newPassword)`

Zmienia hasło dla określonego użytkownika.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample37.cs)]

### `WebSecurity.ConfirmAccount(accountConfirmationToken)`

Potwierdza konto przy użyciu tokenu potwierdzenia konta.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample38.cs)]

### `WebSecurity.CreateAccount(userName, password`  
 `[, requireConfirmationToken = true|false])`

Tworzy nowe konto użytkownika z określoną nazwą użytkownika i hasłem. Aby wymagać tokenu potwierdzenia, należy przekazać wartość true dla *requireConfirmationToken.*

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample39.cs)]

### `WebSecurity.CurrentUserId`

Pobiera identyfikator liczby całkowitej dla aktualnie zalogowanego użytkownika.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample40.cs)]

### `WebSecurity.CurrentUserName`

Pobiera nazwę aktualnie zalogowanego użytkownika.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample41.cs)]

### `WebSecurity.GeneratePasswordResetToken(username`  
 `[, tokenExpirationInMinutesFromNow])`

Generuje token resetowania hasła, który można wysłać w wiadomości e-mail do użytkownika, aby użytkownik mógł zresetować hasło.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample42.cs)]

### `WebSecurity.GetUserId(userName)`

Zwraca identyfikator użytkownika z nazwy użytkownika.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample43.cs)]

### `WebSecurity.IsAuthenticated`

Zwraca wartość PRAWDA, jeśli bieżący użytkownik jest zalogowany.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample44.cs)]

### `WebSecurity.IsConfirmed(userName)`

Zwraca wartość PRAWDA, jeśli użytkownik został potwierdzony (na przykład za pośrednictwem wiadomości e-mail z potwierdzeniem).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample45.cs)]

### `WebSecurity.IsCurrentUser(userName)`

Zwraca wartość PRAWDA, jeśli nazwa bieżącego użytkownika jest zgodna z określoną nazwą użytkownika.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample46.cs)]

### `WebSecurity.Login(userName,password[, persistCookie])`

Rejestruje użytkownika w programie, ustawiając token uwierzytelniania w pliku cookie.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample47.cs)]

### `WebSecurity.Logout()`

Wylogowuje użytkownika przez usunięcie pliku cookie tokenu uwierzytelniania.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample48.css)]

### `WebSecurity.RequireAuthenticatedUser()`

Jeśli użytkownik nie jest uwierzytelniony, ustawia stan HTTP na wartość 401 (Bez autoryzacji).

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample49.css)]

### `WebSecurity.RequireRoles(roles)`

Jeśli bieżący użytkownik nie jest członkiem jednej z określonych ról, ustawia stan HTTP na 401 (nieautoryzowany).

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample50.html)]

### `WebSecurity.RequireUser(userId)`  
`WebSecurity.RequireUser(userName)`

Jeśli bieżący użytkownik nie jest użytkownikiem *określonym przez użytkownika*, ustawia stan HTTP na 401 (nieautoryzowany).

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample51.css)]

### `WebSecurity.ResetPassword(passwordResetToken,newPassword)`

Jeśli token resetowania hasła jest prawidłowy, program zmieni hasło użytkownika na nowe hasło.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample52.css)]

<a id="Data"></a>
## <a name="data"></a>Dane

### `Database.Execute(SQLstatement [,parameters]`

Wykonuje element *sqlstateing* (z opcjonalnymi parametrami), taki jak INSERT, DELETE lub Update i zwraca liczbę rekordów, których to dotyczy.

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample53.sql)]

### `Database.GetLastInsertId()`

Zwraca kolumnę tożsamości z ostatnio wstawionego wiersza.

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample54.sql)]

### `Database.Open(filename)`  
`Database.Open(connectionStringName)`

Otwiera określony plik bazy danych lub bazę danych określoną przy użyciu nazwanych parametrów połączenia z pliku *Web. config* .

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample55.cs)]

### `Database.OpenConnectionString(connectionString)`

Otwiera bazę danych przy użyciu parametrów połączenia. (Ten kontrast `Database.Open`, który używa nazwy parametrów połączenia).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample56.cs)]

### `Database.Query(SQLstatement[,parameters])`

Wysyła zapytanie do bazy danych przy użyciu *sqlstateion* (opcjonalnie przekazując parametry) i zwraca wyniki jako kolekcję.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample57.html)]

### `Database.QuerySingle(SQLstatement [, parameters])`

Wykonuje element *sqlstateing* (z opcjonalnymi parametrami) i zwraca jeden rekord.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample58.cs)]

### `Database.QueryValue(SQLstatement [, parameters])`

Wykonuje element *sqlstateing* (z opcjonalnymi parametrami) i zwraca pojedynczą wartość.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample59.cs)]

<a id="Helpers"></a>
## <a name="helpers"></a>Pomocnicy

### `Analytics.GetGoogleHtml(webPropertyId)`

Renderuje kod JavaScript usługi Google Analytics dla podanego identyfikatora.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample60.js)]

### `Analytics.GetStatCounterHtml(project,security)`

Renderuje kod JavaScript StatCounter Analytics dla określonego projektu.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample61.css)]

### `Analytics.GetYahooHtml(account)`

Renderuje kod JavaScript usługi Yahoo Analytics dla określonego konta.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample62.js)]

### `Bing.SearchBox([boxWidth])`

Przekazuje wyszukiwanie do usługi Bing. Aby określić lokację do przeszukania i tytuł pola wyszukiwania, można ustawić właściwości `Bing.SiteUrl` i `Bing.SiteTitle`. Zwykle te właściwości są ustawiane na stronie *\_AppStart* .

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample63.html)]

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample64.cshtml)]

### `Chart(width,height [, template] [, templatePath])`

Inicjuje wykres.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample65.cshtml)]

### `Chart.AddLegend([title] [, name])`

Dodaje legendę do wykresu.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample66.cshtml)]

### `Chart.AddSeries([name] [, chartType] [, chartArea]`  
 `[, axisLabel] [, legend] [, markerStep] [, xValue]`  
 `[, xField] [, yValues] [, yFields] [, options])`

Dodaje serię wartości do wykresu.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample67.cshtml)]

### `Crypto.Hash(string [, algorithm])`  
`Crypto.Hash(bytes [, algorithm])`

Zwraca wartość skrótu dla określonych danych. Domyślny algorytm to `sha256`.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample68.html)]

### `Facebook.LikeButton(href [, buttonLayout] [, showFaces] [, width] [, height]`   
 `[, action] [, font] [, colorScheme] [, refLabel])`

Umożliwia użytkownikom serwisu Facebook nawiązywanie połączenia z stronami.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample69.js)]

### `FileUpload.GetHtml([initialNumberOfFiles] [, allowMoreFilesToBeAdded]`  
 `[, includeFormTag] [, addText] [, uploadText])`

Renderuje interfejs użytkownika do przekazywania plików.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample70.html)]

### `GamerCard.GetHtml(gamerTag)`

Renderuje określony tag usługi Xbox graczy.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample71.js)]

### `Gravatar.GetHtml(email [, imageSize] [, defaultImage] [, rating]`  
 `[, imageExtension] [, attributes])`

Renderuje obraz skonfigurowali Gravatar dla podanego adresu e-mail.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample72.css)]

### `Json.Encode(object)`

Konwertuje obiekt danych na ciąg w formacie JavaScript Object Notation (JSON).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample73.cs)]

### `Json.Decode(string)`

Konwertuje ciąg wejściowy zakodowany w formacie JSON na obiekt danych, który można wykonać iterację lub wstawić do bazy danych.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample74.cs)]

### `LinkShare.GetHtml(pageTitle[, pageLinkBack] [, twitterUserName]`  
 `[, additionalTweetText] [, linkSites])`

Renderuje linki do sieci społecznościowych przy użyciu określonego tytułu i opcjonalnego adresu URL.

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample75.xml)]

### `ModelStateDictionary.AddError(key, errorMessage)`

Kojarzy komunikat o błędzie z polem formularza. Użyj pomocnika `ModelState`, aby uzyskać dostęp do tego elementu członkowskiego.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample76.cs)]

### `ModelStateDictionary.AddFormError(errorMessage)`

Kojarzy komunikat o błędzie z formularzem. Użyj pomocnika `ModelState`, aby uzyskać dostęp do tego elementu członkowskiego.

[!code-powershell[Main](asp-net-web-pages-api-reference/samples/sample77.ps1)]

### `ModelStateDictionary.IsValid`

Zwraca wartość true, jeśli nie występują błędy walidacji. Użyj pomocnika `ModelState`, aby uzyskać dostęp do tego elementu członkowskiego.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample78.cs)]

### `ObjectInfo.Print(value [, depth] [, enumerationLength])`

Renderuje właściwości i wartości obiektu i wszystkich obiektów podrzędnych.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample79.css)]

### `Recaptcha.GetHtml([, publicKey] [, theme] [, language] [, tabIndex])`

Renderuje test weryfikacyjny reCAPTCHA.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample80.css)]

### `ReCaptcha.PublicKey`  
 `ReCaptcha.PrivateKey`

Ustawia klucze publiczne i prywatne dla usługi reCAPTCHA. Zwykle te właściwości są ustawiane na stronie *\_AppStart* .

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample81.css)]

### `ReCaptcha.Validate([, privateKey])`

Zwraca wynik testu reCAPTCHA.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample82.cs)]

### `ServerInfo.GetHtml()`

Renderuje informacje o stanie dotyczące stron sieci Web ASP.NET.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample83.cshtml)]

### `Twitter.Profile(twitterUserName)`

Renderuje strumień usługi Twitter dla określonego użytkownika.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample84.js)]

### `Twitter.Search(searchQuery)`

Renderuje strumień usługi Twitter dla określonego tekstu wyszukiwania.

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample85.xml)]

### `Video.Flash(filename [, width, height])`

Renderuje odtwarzacz wideo Flash dla określonego pliku z opcjonalną szerokością i wysokością.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample86.cshtml)]

### `Video.MediaPlayer(filename [, width, height])`

Renderuje program Windows Media Player dla określonego pliku z opcjonalną szerokością i wysokością.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample87.cshtml)]

### `Video.Silverlight(filename, width, height)`

Renderuje odtwarzacz Silverlight dla określonego pliku *XAP* z wymaganą szerokością i wysokością.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample88.cshtml)]

### `WebCache.Get(key)`

Zwraca obiekt określony przez *klucz*lub wartość null, jeśli nie można odnaleźć obiektu.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample89.cs)]

### `WebCache.Remove(key)`

Usuwa obiekt określony przez *klucz* z pamięci podręcznej.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample90.cs)]

### `WebCache.Set(key, value [, minutesToCache] [, slidingExpiration])`

Umieszcza *wartość* w pamięci podręcznej pod nazwą określoną przez *klucz*.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample91.html)]

### `WebGrid(data)`

Tworzy nowy obiekt `WebGrid` przy użyciu danych z zapytania.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample92.cs)]

### `WebGrid.GetHtml()`

Renderuje znaczniki, aby wyświetlić dane w tabeli HTML.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample93.html)]

### `WebGrid.Pager()`

Renderuje moduł stronicowania dla obiektu `WebGrid`.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample94.html)]

### `WebImage(path)`

Ładuje obraz z określonej ścieżki.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample95.cs)]

### `WebImage.AddImagesWatermark(image)`

Dodaje określony obraz jako znak wodny.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample96.cs)]

### `WebImage.AddTextWatermark(text)`

Dodaje określony tekst do obrazu.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample97.cs)]

### `WebImage.FlipHorizontal()`  
`WebImage.FlipVertical()`

Przerzuca obraz w poziomie lub w pionie.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample98.css)]

### `WebImage.GetImageFromRequest()`

Ładuje obraz, gdy obraz zostanie opublikowany na stronie podczas przekazywania pliku.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample99.cs)]

### `WebImage.Resize(width,height)`

Zmienia rozmiar obrazu.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample100.css)]

### `WebImage.RotateLeft()`  
`WebImage.RotateRight()`

Obraca obraz w lewo lub w prawo.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample101.css)]

### `WebImage.Save(path [, imageFormat])`

Zapisuje obraz do określonej ścieżki.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample102.js)]

### `WebMail.Password`

Ustawia hasło dla serwera SMTP. Zwykle tę właściwość można ustawić na stronie *\_AppStart* .

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample103.cs)]

### `WebMail.Send(to, subject, body [, from] [, cc] [, filesToAttach] [, isBodyHtml]`  
 `[, additionalHeaders])`

Wysyła wiadomość e-mail.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample104.css)]

### `WebMail.SmtpServer`

Ustawia nazwę serwera SMTP. Zwykle tę właściwość można ustawić na stronie *\_AppStart* .

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample105.html)]

### `WebMail.UserName`

Ustawia nazwę użytkownika dla serwera SMTP. Zwykle należy ustawić tę właściwość na stronie *\_AppStart* .

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample106.html)]

<a id="Validation"></a>
## <a name="validation"></a>Walidacja

### `Html.ValidationMessage(field)`

v Renderuje komunikat o błędzie walidacji dla określonego pola.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample107.cshtml)]

### `Html.ValidationSummary([message])`

v Wyświetla listę wszystkich błędów walidacji.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample108.cshtml)]

### `Validation.Add(field, validationType)`

v Rejestruje element danych wejściowych użytkownika dla określonego typu walidacji.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample109.js)]

### `Validation.ClassFor(field)`

v Dynamicznie renderuje atrybuty klasy CSS dla weryfikacji po stronie klienta, aby można było sformatować komunikaty o błędach walidacji. (Wymaga odwołania do odpowiednich bibliotek skryptu klienta i zdefiniowania klas CSS).

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample110.html)]

### `Validation.For(field)`

v Włącza weryfikację po stronie klienta dla pola danych wejściowych użytkownika. (Wymaga odwołania do odpowiednich bibliotek skryptu klienta).

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample111.html)]

### `Validation.IsValid()`

v Zwraca wartość PRAWDA, jeśli wszystkie elementy danych wejściowych użytkownika, które są registred do walidacji, zawierają prawidłowe wartości.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample112.cs)]

### `Validation.RequireField(field[, errorMessage])`

v Określa, że użytkownicy muszą podać wartość dla elementu danych wejściowych użytkownika.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample113.cs)]

### `Validation.RequireFields(field1[, field12, field3, ...])`

v Określa, że użytkownicy muszą podać wartości dla każdego elementu danych wejściowych użytkownika. Ta metoda nie pozwala na określenie niestandardowego komunikatu o błędzie.

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

v Określa test weryfikacyjny przy użyciu metody `Validation.Add`.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample115.js)]
