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
# <a name="http-cookies-in-aspnet-web-api"></a>Pliki cookie protokołu HTTP we wzorcu ASP.NET Web API

według [Jan Wasson](https://github.com/MikeWasson)

W tym temacie opisano sposób wysyłania i odbierania plików cookie protokołu HTTP w interfejsie API sieci Web.

## <a name="background-on-http-cookies"></a>Tło dla plików cookie HTTP

Ta sekcja zawiera krótkie omówienie sposobu implementacji plików cookie na poziomie protokołu HTTP. Aby uzyskać szczegółowe informacje, zapoznaj się z [dokumentem RFC 6265](http://tools.ietf.org/html/rfc6265).

Plik cookie to element danych wysyłany przez serwer w odpowiedzi HTTP. Klient (opcjonalnie) przechowuje plik cookie i zwraca go na kolejne żądania. Umożliwia to klientowi i serwerowi udostępnianie stanu. Aby ustawić plik cookie, serwer zawiera nagłówek Set-cookie w odpowiedzi. Format pliku cookie to para nazwa-wartość z opcjonalnymi atrybutami. Na przykład:

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

Oto przykład z atrybutami:

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

Aby zwrócić plik cookie na serwer, klient zawiera nagłówek pliku cookie w późniejszych żądaniach.

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

Odpowiedź HTTP może zawierać wiele nagłówków Set-cookie.

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

Klient zwraca wiele plików cookie przy użyciu jednego nagłówka pliku cookie.

[!code-console[Main](http-cookies/samples/sample5.cmd)]

Zakres i czas trwania pliku cookie są kontrolowane przez następujące atrybuty w nagłówku Set-Cookie:

- **Domena**: informuje klienta, która domena powinna odebrać plik cookie. Na przykład jeśli domena jest "example.com", klient zwraca plik cookie do każdej poddomeny example.com. Jeśli nie zostanie określony, domena jest serwerem pochodzenia.
- **Ścieżka**: ogranicza plik cookie do określonej ścieżki w domenie. Jeśli nie zostanie określony, zostanie użyta ścieżka identyfikatora URI żądania.
- **Wygasa**: ustawia datę wygaśnięcia pliku cookie. Klient usuwa plik cookie po jego wygaśnięciu.
- **Max-age**: ustawia maksymalny wiek pliku cookie. Klient usuwa plik cookie po osiągnięciu maksymalnego wieku.

Jeśli ustawiono zarówno `Expires`, jak i `Max-Age`, `Max-Age` ma pierwszeństwo. Jeśli żadna z tych wartości nie zostanie ustawiona, klient usunie plik cookie po zakończeniu bieżącej sesji. (Dokładne znaczenie "Session" jest określane przez agenta użytkownika.)

Należy jednak pamiętać, że klienci mogą ignorować pliki cookie. Na przykład użytkownik może wyłączyć pliki cookie ze względów zachowania poufności informacji. Klienci mogą usuwać pliki cookie przed ich wygaśnięciem lub ograniczyć liczbę przechowywanych plików cookie. Ze względu na prywatność klienci często odrzucają pliki cookie "inne firmy", w których domena nie jest zgodna z serwerem źródłowym. W krótkim przypadku serwer nie powinien opierać się na przywracaniu plików cookie, które ustawia.

## <a name="cookies-in-web-api"></a>Pliki cookie w internetowym interfejsie API

Aby dodać plik cookie do odpowiedzi HTTP, Utwórz wystąpienie **CookieHeaderValue** , które reprezentuje plik cookie. Następnie Wywołaj metodę rozszerzenia **Addcookiess** , która jest zdefiniowana w **systemie .NET. http. HttpResponseHeadersExtensions** , aby dodać plik cookie.

Na przykład poniższy kod dodaje plik cookie w ramach akcji kontrolera:

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

Zwróć uwagę, że **addcookies** pobiera tablicę wystąpień **CookieHeaderValue** .

Aby wyodrębnić pliki cookie z żądania klienta, wywołaj metodę **Getcookiess** :

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

**CookieHeaderValue** zawiera kolekcję wystąpień **CookieState** . Każdy **CookieState** reprezentuje jeden plik cookie. Użyj metody Indexer, aby uzyskać **CookieState** według nazwy, jak pokazano.

## <a name="structured-cookie-data"></a>Strukturalne dane pliku cookie

Wiele przeglądarek ogranicza liczbę plików cookie, które będą&#8212;przechowywać zarówno całkowitą liczbę, jak i liczbę dla każdej domeny. W związku z tym może być przydatne umieszczenie danych strukturalnych w jednym pliku cookie zamiast ustawiania wielu plików cookie.

> [!NOTE]
> Specyfikacja RFC 6265 nie definiuje struktury danych plików cookie.

Korzystając z klasy **CookieHeaderValue** , można przekazać listę par nazwa-wartość dla danych plików cookie. Te pary nazwa-wartość są kodowane jako dane formularza kodowane przy użyciu adresu URL w nagłówku Set-Cookie:

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

Poprzedni kod generuje następujący nagłówek-cookie:

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

Klasa **CookieState** zapewnia metodę indeksatora, która odczytuje wartości podrzędne z pliku cookie w komunikacie żądania:

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a>Przykład: Ustawianie i pobieranie plików cookie w programie obsługi komunikatów

W poprzednich przykładach pokazano, jak używać plików cookie z poziomu kontrolera interfejsu API sieci Web. Innym rozwiązaniem jest użycie [obsługi komunikatów](http-message-handlers.md). Procedury obsługi komunikatów są wywoływane wcześniej w potoku niż kontrolery. Program obsługi komunikatów może odczytywać pliki cookie z żądania, zanim żądanie osiągnie kontroler lub doda pliki cookie do odpowiedzi po wygenerowaniu przez kontroler odpowiedzi.

![](http-cookies/_static/image2.png)

Poniższy kod przedstawia procedurę obsługi komunikatów na potrzeby tworzenia identyfikatorów sesji. Identyfikator sesji jest przechowywany w pliku cookie. Program obsługi sprawdza żądanie dotyczące pliku cookie sesji. Jeśli żądanie nie zawiera pliku cookie, program obsługi generuje nowy identyfikator sesji. W obu przypadkach program obsługi przechowuje identyfikator sesji w zbiorze właściwości **HttpRequestMessage. Properties** . Dodaje również plik cookie sesji do odpowiedzi HTTP.

Ta implementacja nie sprawdza, czy identyfikator sesji klienta został faktycznie wystawiony przez serwer. Nie używaj go jako formy uwierzytelniania. W punkcie przykładu można wyświetlić zarządzanie plikami cookie HTTP.

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

Kontroler może uzyskać identyfikator sesji z zbioru właściwości **HttpRequestMessage. Properties** .

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
