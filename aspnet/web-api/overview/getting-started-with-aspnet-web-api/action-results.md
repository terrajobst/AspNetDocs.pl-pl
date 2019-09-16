---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: Wyniki akcji w interfejsie Web API 2 — ASP.NET 4. x
author: MikeWasson
description: Opisuje sposób, w jaki interfejs API sieci Web ASP.NET konwertuje wartość zwracaną z akcji kontrolera na komunikat odpowiedzi HTTP w ASP.NET 4. x.
ms.author: riande
ms.date: 02/03/2014
ms.custom: seoapril2019
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: f00ac0db453053e53d6d6942dd1557b409f4167b
ms.sourcegitcommit: 4b324a11131e38f920126066b94ff478aa9927f8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2019
ms.locfileid: "70985837"
---
# <a name="action-results-in-web-api-2"></a>Wyniki akcji we wzorcu Web API 2

[!INCLUDE[](~/includes/coreWebAPI.md)]

W tym temacie opisano sposób, w jaki interfejs API sieci Web ASP.NET konwertuje wartość zwracaną z akcji kontrolera na komunikat odpowiedzi HTTP.

Akcja kontrolera interfejsu API sieci Web może zwracać dowolne z następujących elementów:

1. void
2. **HttpResponseMessage**
3. **IHttpActionResult**
4. Inny typ

W zależności od tego, które z nich są zwracane, interfejs API sieci Web używa innego mechanizmu do tworzenia odpowiedzi HTTP.

| Typ zwracany | Jak interfejs API sieci Web tworzy odpowiedź |
| --- | --- |
| void | Zwróć pustą 204 (brak zawartości) |
| **HttpResponseMessage** | Konwertuj bezpośrednio na komunikat odpowiedzi HTTP. |
| **IHttpActionResult** | Wywołaj **wywoływanie ExecuteAsync** , aby utworzyć **HttpResponseMessage**, a następnie przekonwertuj na komunikat odpowiedzi HTTP. |
| Inny typ | Zapisz zserializowaną wartość zwrotną w treści odpowiedzi. Zwróć 200 (OK). |

W pozostałej części tego tematu opisano każdą opcję bardziej szczegółowo.

## <a name="void"></a>void

Jeśli zwracanym typem jest `void`, interfejs API sieci Web po prostu zwraca pustą odpowiedź HTTP z kodem stanu 204 (brak zawartości).

Przykładowy kontroler:

[!code-csharp[Main](action-results/samples/sample1.cs)]

Odpowiedź HTTP:

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a>HttpResponseMessage

Jeśli akcja zwraca [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), internetowy interfejs API konwertuje wartość zwracaną bezpośrednio na komunikat odpowiedzi HTTP, używając właściwości obiektu **HttpResponseMessage** do wypełnienia odpowiedzi.

Ta opcja zapewnia dużą kontrolę nad komunikatem odpowiedzi. Na przykład następująca akcja kontrolera ustawia nagłówek Cache-Control.

[!code-csharp[Main](action-results/samples/sample3.cs)]

Reakcji

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

W przypadku przekazania modelu domeny do metody " **Xmlresponse** " interfejs API sieci Web używa programu [formatującego multimedia](../formats-and-model-binding/media-formatters.md) , aby napisać serializowany model do treści odpowiedzi.

[!code-csharp[Main](action-results/samples/sample5.cs)]

Interfejs API sieci Web używa nagłówka Accept w żądaniu do wybrania programu formatującego. Aby uzyskać więcej informacji, zobacz [negocjacje zawartości](../formats-and-model-binding/content-negotiation.md).

## <a name="ihttpactionresult"></a>IHttpActionResult

Interfejs **IHttpActionResult** został wprowadzony w interfejsie Web API 2. Zasadniczo definiuje fabrykę **HttpResponseMessage** . Poniżej przedstawiono niektóre zalety korzystania z interfejsu **IHttpActionResult** :

- Upraszcza [testowanie jednostkowe](../testing-and-debugging/unit-testing-controllers-in-web-api.md) kontrolerów.
- Przenosi wspólną logikę do tworzenia odpowiedzi HTTP w oddzielnych klasach.
- Sprawia, że zamiarowa akcja kontrolera jest przejrzysta, ukrywając szczegóły niskiego poziomu konstruowania odpowiedzi.

**IHttpActionResult** zawiera jedną metodę **wywoływanie ExecuteAsync**, która asynchronicznie tworzy wystąpienie **HttpResponseMessage** .

[!code-csharp[Main](action-results/samples/sample6.cs)]

Jeśli akcja kontrolera zwraca **IHttpActionResult**, interfejs API sieci Web wywołuje metodę **wywoływanie ExecuteAsync** , aby utworzyć **HttpResponseMessage**. Następnie konwertuje **HttpResponseMessage** na komunikat odpowiedzi HTTP.

Oto prosta implementacja **IHttpActionResult** , która tworzy odpowiedź w postaci zwykłego tekstu:

[!code-csharp[Main](action-results/samples/sample7.cs)]

Przykładowa akcja kontrolera:

[!code-csharp[Main](action-results/samples/sample8.cs)]

Reakcji

[!code-console[Main](action-results/samples/sample9.cmd)]

Często używane są implementacje **IHttpActionResult** zdefiniowane w przestrzeni nazw **[System. Web. http. results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** . Klasa **ApiController** definiuje metody pomocnika, które zwracają te wbudowane wyniki akcji.

W poniższym przykładzie, jeśli żądanie nie jest zgodne z istniejącym IDENTYFIKATORem produktu, kontroler wywoła [ApiController. NOTFOUND](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) , aby utworzyć odpowiedź 404 (nie znaleziono). W przeciwnym razie kontroler wywoła [ApiController. ok](https://msdn.microsoft.com/library/dn314591.aspx), co spowoduje utworzenie odpowiedzi 200 (ok) zawierającej produkt.

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a>Inne typy zwracane

Dla wszystkich innych typów zwracanych interfejs API sieci Web używa programu [formatującego multimedia](../formats-and-model-binding/media-formatters.md) do serializacji wartości zwracanej. Interfejs API sieci Web zapisuje zserializowaną wartość w treści odpowiedzi. Kod stanu odpowiedzi to 200 (OK).

[!code-csharp[Main](action-results/samples/sample11.cs)]

Wadą tego podejścia jest to, że nie można bezpośrednio zwrócić kodu błędu, takiego jak 404. Można jednak zgłosić **HttpResponseException** dla kodów błędów. Aby uzyskać więcej informacji, zobacz [Obsługa wyjątków w interfejsie API sieci Web ASP.NET](../error-handling/exception-handling.md).

Interfejs API sieci Web używa nagłówka Accept w żądaniu do wybrania programu formatującego. Aby uzyskać więcej informacji, zobacz [negocjacje zawartości](../formats-and-model-binding/content-negotiation.md).

Przykładowe żądanie

[!code-console[Main](action-results/samples/sample12.cmd)]

Przykładowa odpowiedź

[!code-console[Main](action-results/samples/sample13.cmd)]
