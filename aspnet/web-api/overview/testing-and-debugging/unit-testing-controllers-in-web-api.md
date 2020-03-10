---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: Kontrolery testowania jednostek w programie ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: W tym temacie opisano niektóre konkretne techniki kontrolerów testowania jednostek w interfejsie Web API 2. Przed przeczytaniem tego tematu warto zapoznać się z jednostką samouczka...
ms.author: riande
ms.date: 06/11/2014
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: cdb1700537021e276669de1a9e0330a62659746c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554993"
---
# <a name="unit-testing-controllers-in-aspnet-web-api-2"></a>Kontrolery testów jednostkowych we wzorcu ASP.NET Web API 2

według [Jan Wasson](https://github.com/MikeWasson)

> W tym temacie opisano niektóre konkretne techniki kontrolerów testowania jednostek w interfejsie Web API 2. Przed zapisaniem tego tematu warto przeczytać artykuł dotyczący [testowania jednostkowego ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), który pokazuje, jak dodać projekt jednostki testowej do rozwiązania.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - Internetowy interfejs API 2
> - [MOQ](https://github.com/Moq) 4.5.30

> [!NOTE]
> Używam MOQ, ale to samo pomysł odnosi się do każdej struktury imitacji. MOQ 4.5.30 (i nowsze) obsługuje program Visual Studio 2017, Roslyn i .NET 4,5 i nowsze wersje.

Typowym wzorcem w testach jednostkowych jest &quot;porządkowanie&quot;z założeniami:

- Porządkowanie: Skonfiguruj wszystkie wymagania wstępne dla testu do uruchomienia.
- Działanie: wykonaj test.
- Potwierdzenie: Sprawdź, czy test zakończył się pomyślnie.

W kroku Rozmieść często będziesz używać obiektów imitacji lub szczątkowych. Pozwala to zminimalizować liczbę zależności, dlatego test koncentruje się na testowaniu jednego elementu.

Oto kilka rzeczy, które należy wykonać test jednostkowy na kontrolerach interfejsu API sieci Web:

- Akcja zwraca poprawny typ odpowiedzi.
- Nieprawidłowe parametry zwracają poprawną odpowiedź na błąd.
- Akcja wywołuje poprawną metodę dla repozytorium lub warstwy usług.
- Jeśli odpowiedź zawiera model domeny, Sprawdź typ modelu.

Są to niektóre ogólne elementy, które należy przetestować, ale te szczegóły zależą od implementacji kontrolera. W szczególności czynimy dużą różnicą, czy akcje kontrolera zwracają **HttpResponseMessage** lub **IHttpActionResult**. Aby uzyskać więcej informacji na temat tych typów wyników, zobacz [wyniki akcji w interfejsie Web API 2](../getting-started-with-aspnet-web-api/action-results.md).

## <a name="testing-actions-that-return-httpresponsemessage"></a>Akcje testowania zwracające HttpResponseMessage

Oto przykład kontrolera, którego akcje zwracają **HttpResponseMessage**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

Zwróć uwagę na to, że kontroler używa iniekcji zależności, aby wstrzyknąć `IProductRepository`. Sprawia, że kontroler jest bardziej weryfikowalne, ponieważ można wstrzyknąć repozytorium makiety. Poniższy test jednostkowy sprawdza, czy metoda `Get` zapisuje `Product` do treści odpowiedzi. Załóżmy, że `repository` jest `IProductRepository`a.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

Ważne jest, aby ustawić **żądanie** i **konfigurację** na kontrolerze. W przeciwnym razie test zakończy się niepowodzeniem z **ArgumentNullException** lub **InvalidOperationException**.

## <a name="testing-link-generation"></a>Testowanie generacji linków

Metoda `Post` wywołuje **UrlHelper. link** , aby utworzyć linki w odpowiedzi. W teście jednostkowym wymagana jest nieco większa konfiguracja:

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

Klasa **UrlHelper** wymaga adresu URL żądania i danych tras, dlatego test musi określić wartości. Inną opcją jest makieta lub szczątkowa **UrlHelper**. W tym podejściu zastąpisz wartość domyślną [ApiController. URL](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) z wersją makiety lub szczątkową zwracającą wartość stałą.

Ponownie napiszemy test przy użyciu struktury [MOQ](https://github.com/Moq) . Zainstaluj pakiet NuGet `Moq` w projekcie testowym.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

W tej wersji nie trzeba konfigurować żadnych danych tras, ponieważ **UrlHelper** makiety zwraca ciąg stały.

## <a name="testing-actions-that-return-ihttpactionresult"></a>Akcje testowania zwracające IHttpActionResult

W interfejsie Web API 2 akcja kontrolera może zwracać **IHttpActionResult**, która jest analogiczna do **ACTIONRESULT** w ASP.NET MVC. Interfejs **IHttpActionResult** definiuje wzorzec poleceń służący do tworzenia odpowiedzi HTTP. Zamiast bezpośredniego tworzenia odpowiedzi kontroler zwraca **IHttpActionResult**. Później potok wywoła **IHttpActionResult** do utworzenia odpowiedzi. Takie podejście ułatwia pisanie testów jednostkowych, ponieważ można pominąć wiele konfiguracji, które są potrzebne do **HttpResponseMessage**.

Oto przykładowy kontroler, którego akcje zwracają **IHttpActionResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

Ten przykład pokazuje niektóre typowe wzorce przy użyciu **IHttpActionResult**. Zobaczmy, jak przeprowadzić testy jednostkowe.

### <a name="action-returns-200-ok-with-a-response-body"></a>Akcja zwraca 200 (OK) z treścią odpowiedzi

Metoda `Get` wywołuje `Ok(product)`, jeśli produkt zostanie znaleziony. W teście jednostkowym upewnij się, że zwracany typ to **OkNegotiatedContentResult** , a zwrócony produkt ma prawidłowy identyfikator.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

Zwróć uwagę, że test jednostkowy nie wykonuje wyniku akcji. Można założyć, że wyniki akcji powodują poprawne utworzenie odpowiedzi HTTP. (Dlatego, że struktura internetowego interfejsu API ma własne testy jednostkowe!)

### <a name="action-returns-404-not-found"></a>Akcja zwraca 404 (nie znaleziono)

Metoda `Get` wywołuje `NotFound()`, jeśli produkt nie zostanie znaleziony. W takim przypadku test jednostkowy sprawdza tylko, czy typem zwracanym jest **NotFoundResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a>Akcja zwraca 200 (OK) bez treści odpowiedzi

Metoda `Delete` wywołuje `Ok()` do zwrócenia pustej odpowiedzi HTTP 200. Podobnie jak w poprzednim przykładzie, test jednostkowy sprawdza zwracany typ, w tym przypadku **OkResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a>Akcja zwraca 201 (utworzono) z nagłówkiem lokalizacji

Metoda `Post` wywołuje `CreatedAtRoute`, aby zwrócić odpowiedź HTTP 201 z identyfikatorem URI w nagłówku lokalizacji. W teście jednostkowym Sprawdź, czy akcja ustawia poprawne wartości routingu.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a>Akcja zwraca kolejną 2xx z treścią odpowiedzi

Metoda `Put` wywołuje `Content` do zwrócenia odpowiedzi HTTP 202 (zaakceptowane) z treścią odpowiedzi. Ten przypadek jest podobny do zwraca 200 (OK), ale test jednostkowy powinien również sprawdzić kod stanu.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a>Dodatkowe materiały

- [Imitacja Entity Framework, gdy testy jednostkowe ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- [Pisanie testów dla usługi internetowego interfejsu API ASP.NET](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (wpis w blogu przez Youssef Moussaoui).
- [Debugowanie interfejsu API sieci Web ASP.NET z debugerem tras](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
